---
title: "PongBreak"
excerpt: "A networked mashup of Pong and Breakout."

header:
  image: /assets/images/portfolio/2015/pongbreak/feature.png
  teaser: /assets/images/portfolio/2015/pongbreak/feature.png

language: "C++"
year: "2015"
portfolio: "true"

sidebar:
  - text: "## 2015, C++ ##"


  - title: "Description"

    text: "A networked mashup of Pong and Breakout. Uses RakNet and SFML."


  - title: "Outcomes"

    text: "CMake.


    RakNet.


    TCP/UDP.


    Client/Server model."


  - title: "Links"

  - button: Repository
    link: https://github.com/tyskwo/RakNet_PongBreak
    icon: github

  - button: Executable
    link: /assets/downloads/2015/pongbreak/PongBreak.zip
    icon: download
---

This project was an exercise in networking for games: using RakNet, there is a dedicated server that clients can connect to. Concepts like packet loss, differences between TCP/UDP, and different ways to handle network lag and syncing was covered over the course of the project.


### Architecture

_PongBreak_ uses an authoritative server which simulates the game and sends the states to its clients to display to the players. This rids the need of temporal delay. However, there is interpolation to account for the time difference between the clients and the server.

![pongbreak-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/pongbreak/intro.gif){: .align-center}

##### Server

The server contains a 2D array of client pairs, giving it the capability of running multiple games at the same time. It has an update loop, where it updates each game individually, and sends the new game state (a struct called `GameInfo`) to each client. Upon initialization, it creates all of the `Game` objects, as well as opens up connection ports to listen on. Upon a game ending, it waits for confirmation from a client to restart, or closes the connections and frees up the client slots if the clients disconnect.

The clients and server send packets back and forth a couple times each second. In order to reduce the amount of information flowing over the network, the info stored in each packet is as concise as possible, saving space. Here's the method that reads the packets' information:

{% highlight cpp %}
void Server::getPackets()
{
	for (p = mpServer->Receive(); p; mpServer->DeallocatePacket(p), p = mpServer->Receive())
	{
		packetIdentifier = GetPacketIdentifier(p);

        //obviously all of these cases are filled out with the correct logic
		switch (packetIdentifier)
		{
		case ID_DISCONNECTION_NOTIFICATION

		case ID_INCOMPATIBLE_PROTOCOL_VERSION

		case ID_CONNECTED_PING
		case ID_UNCONNECTED_PING

		case ID_CONNECTION_LOST

		case ID_NEW_INCOMING_CONNECTION

		case ID_SEND_PADDLE_DATA

		case ID_START_GAME

		default:
			printf("%s\n", p->data);
			break;
		}
	}
}
{% endhighlight %}

The magic lies in `GetPacketIdentifier`, which gets the header of the packet. This allows us to understand what type of packet it is (and what we should do with its contents). For us, the packet identifiers are a simple enum.

{% highlight cpp %}
unsigned char Server::GetPacketIdentifier(RakNet::Packet *p)
{
	if (p == 0)
		return 255;

	if ((unsigned char)p->data[0] == ID_TIMESTAMP)
	{
		RakAssert(p->length > sizeof(RakNet::MessageID) + sizeof(RakNet::Time));
		return (unsigned char)p->data[sizeof(RakNet::MessageID) + sizeof(RakNet::Time)];
	}
	else
		return (unsigned char)p->data[0];
}
{% endhighlight %}

After `update()` is finished, it sends the game info to all of the clients, simply sending a packet to each client:

{% highlight cpp %}
void Server::broadcastGameInfo()
{
	int j = 0;
	for (unsigned int i = 0; i < mClientPairs.size(); i++)
	{
		GameInfo info = mGameInfos[j];
		info.mID = ID_RECIEVE_GAME_INFO;

		mpServer->Send((const char*)&info, sizeof(info), HIGH_PRIORITY, RELIABLE_ORDERED, 0, mGuidPairs[i][0], false);
		mpServer->Send((const char*)&info, sizeof(info), HIGH_PRIORITY, RELIABLE_ORDERED, 0, mGuidPairs[i][1], false);

		if (i % 2 == 1) j++; //we send the same GameInfo to each set of clients
	}
}
{% endhighlight %}


##### Client

The client simply responds to user input, and draws the updated `GameInfo` sent from the server. Upon receiving input from the player, it converts that to data based on the most recent info. For instance, here's how a client would send updated paddle info:

{% highlight cpp %}
if (sf::Keyboard::isKeyPressed(sf::Keyboard::Up))
{
	mpClient->setPaddlePosition(-5.0f);
}

.....


if (mpTimer->shouldUpdate())
{
	if (isFirstConnected())
        sendPaddleData(mGameInfo.lPlayer.x, mGameInfo.lPlayer.y);
	else
		sendPaddleData(mGameInfo.rPlayer.x, mGameInfo.rPlayer.y);
}

.....

void Client::sendPaddleData(float x, float y)
{
	Position pos;
	pos.x = x;
	pos.y = y;
	pos.mID = ID_SEND_PADDLE_DATA;
	mpClient->Send((const char*)&pos, sizeof(pos), HIGH_PRIORITY, RELIABLE_ORDERED, 0, RakNet::UNASSIGNED_SYSTEM_ADDRESS, true);
}
{% endhighlight %}


### Retrospection and Postmortem

This was my first experience in networking, and I am thankful that it was using an industry standard like _RakNet_. It gave me experience in separating server logic from game logic, as well as introduce me to topics like interpolation, replication, and ways to handle network lag like temporal delay. The game itself is simple, but it does a good job in creating an environment where a good amount of data is needed to be sent back and forth.
