---
title: "Shooter State Machine"
excerpt: "1 player AI vs infinite."

header:
  image: /assets/images/portfolio/2015/shooter-state-machine/feature.png
  teaser: /assets/images/portfolio/2015/shooter-state-machine/feature.png

language: "AS3"
year: "2015"


sidebar:
  - text: "## 2015, AS3 ##"


  - title: "Description"

    text: "An exercise in creating state machines: one for the player to survive as long as possible, and one for the enemies to hunt the player down.
"


  - title: "Outcomes"

    text: "Flashpunk.


    State machine AI pattern."
---


This project's goal was to create an AI that could take over for a real human player using a structured state machine. This was a partner project with Drew Matthews, and we created a simple twin-stick shooter to show-off our AI. We used _Flashpunk_ for the underlying engine, which allows us for easy collision detection. Along with the player AI, we also have an enemy state machine, and easy switching between AI and human controls.

![ai-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/shooter-state-machine/nogui.gif){: .align-center}

### The State Machines ###

Both player-types share the same state machine architecture:

{% highlight as3 %}
public class State
{
	protected var mEntity:Entity;
	protected var mStateType:int;

	public function State(ent:Entity)
	{ mEntity = ent; }

	public function update():void {}

	//Used for setting data when entering state
	public function onLoadState(prevState:State):void {}

	//Used for data when leaving state
	public function offLoadState(nextState:State):void {}

	//Returns a state when a condition is met
            //subclasses implement this
	public function callback():State
	{ return null; }

	//Returns the int state type
	public function getStateType():int
	{ return mStateType; }
}
{% endhighlight %}

Each player-type then has different states such as patrol, shoot, seek health, investigate, etc. We then form a state web, making segues between the different states, and what the conditions of the segue are (such as low health, enemy close by, etc.) Each state can offload data to the next state, thus its possible to alter behavior in multiple states and stay consistent (if power-ups were involved, for instance).

![ai-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/shooter-state-machine/example.gif){: .align-center }

### Retrospection and Postmortem ###

This project showed me the possible advantages of using finite state machines over decision or behavior trees. Here, each state, or behavior, is encapsulated and within its own class, allowing it to be independent of any other states and the ability to be hot-plugged based on other factors in the game (or for fast prototyping). On the other hand, behavior/decision trees tend to have their logic and behavior separated, which leads to the system reaching further in the codebase. This could be remedied slightly by having grouping behaviors into categories, but this still isn't ideal: it doesn't allow for individual behaviors to be independent of the system without a good bit of over-engineering. Still, there are advantages of behavior trees too, but that's for another post. Overall, this project went well; I am proud of the result, and I feel it demonstrates a good understanding of how state machines work, and the code itself is clean and concise too.
