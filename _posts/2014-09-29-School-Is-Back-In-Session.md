---
title: "School is Back in Session"
description: "The first (of probably many) life check-ins."
---


Aw man, where did the summer go? It is almost October at this point, and I am back on the banks of Lake Champlain. I realize my last post was way back in February, so I think a little recap is necessary. After finishing [*Orbit*]({{ site.url }}/2014/introducing-Orbit!/), I went on to finish my Game Technology class with similar projects. My final project was a puzzle game, which I brought to iOS as part of the project, and I spent the summer progressing along with it. I am almost at the point of releasing it, and hope to do so before the end of the year. It's called Flip, and has some elements of Tetris and Threes, but it is its complete own idea. I've been getting good feedback about its mechanics, so I'm really excited for it.


Come late August, I made the ugly, then traffic-filled, then beautiful drive from New Jersey to Vermont and settled into my Sophomore year at Champlain. Or so I thought. The Monday after I arrived, I came down with a high fever and the chills, among other things. After a week of trying to fight through it, I realized I was not getting any better. After visiting a couple of doctors, I was diagnosed with viral meningitis. Cue five weeks of back, neck, and joint pain. To put it in perspective of how crippling this disease is, today is the first day I am able to jump without my neck feeling like it will fall off - five weeks since I first caught it. I am still not 100% yet, but I'm getting better everyday.


As for this semester, I am taking two games-related classes: Data Structures and Algorithms and Graphics Programming I. Graphics Programming is similar to Game Tech where we have a project due each week; a little prototype of a game, except Graphics uses XNA and thus C#. Although similar to AS3 and Java in certain aspects, it is another language I now 'know.' What is awesome (yet work-intensive) about Graphics Programming is not only do the projects focus on purely graphic elements of games, but also on the little things that don't seem to fit in their own class (unless there is a Tidbits for Games 320 that I don't know about). Things like menu systems, or supporting different languages, or correctly implementing pause screens - the things that are not polish, but aren't necessary for the games I create in class. It is nice to learn these things without needing to just hack them together and hope it is efficient like in the past.


With a project being due each week for this class, there is a trade-off between efficiency, clarity, and just getting it done. My code has been a little sloppy on some of these projects. I decided to spend the weekend going back to one of my projects and refactor the code. It was not as bad as I thought it was, but I was still able to spruce it up.


My goal was to make the project as neat as possible without tearing my hair out. The only thing that's not to my liking is the main gameplay itself, as I had too many dependencies in the main game class to compartmentalize itself into its own class. Other than that, every other screen displayed is its own class. I cleaned up the language selection code. I probably had close to 100 redundant if statements pertaining to languid; now I have none. The enemies all stem from a base enemy class, as do the menus from a base menu class. The player is its own class. The only thing I didn't do is the actual gameplay itself, but that would require more time than just a weekend. The main class after all of this was said and done went from almost 1100 lines of (crunched) code, to just over 500 (I think something like 512, definitely under half) of properly spaced code. The gameplay is the same, but the code, although it might not be a ton more efficient, is definitely cleaner and much easier to read and follow.


I like toying with new ideas and concepts and creating new prototypes, but it is necessary every now and then to learn new concepts pertaining to actual code organization, and I'm sure I have just scratched the surface. I know there are many many many ways of organizing a game's code, and this little weekend project on a prototype has inspired me to research these methods. Stay tuned, for there may or may not be a blog post in the future pertaining to these methods, or Flip - whichever takes hold of my mind next.
