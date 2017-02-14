---
title: "Minesweeper Update"
description: "The Minesweeper challenge is underway, and I'm making good headway!"
---

I have made good headway before the deadline this coming Friday. The 'brain' is at the point where it is achieving *`94%`* of a perfect score on the given parameters over *`900`* games. It's best was a whopping *`98.9% (45870/46360)`*. Let's dig into the details.

I have a `MineBrain` class that encompasses all of the logic so that it is separate from anything else that might be done in the DLL. In the main class of the DLL, there is a `makeDecision` method that has an `Event` as a parameter. This event is an enum that can be one of four options: `INIT_EVENT, CLEANUP_EVENT, GAME_RESET_EVENT, PICK_A_CELL_EVENT`. Most of the magic happens in the last one.

The brain has three vectors: one to hold all of the revealed tiles in the map, one to hold all of the flagged tiles (or tiles we know are bombs), and one for safe tiles – a stockpile of tiles we know aren't a bomb and aren't revealed. In the`PICK_A_CELL_EVENT`, we first check if it's the first move. If it is, we store a reference to the board (so we don't need to constantly pass it back and forth), and we pick a tile: but not any tile. Although the research isn't conclusive, [this article shows](http://www.minesweeper.info/wiki/Strategy#First_Click) that picking a corner tile is the best bet for games where the first click is a given such as our case. For my brain, I just chose the bottom right as it is the easiest index to return. After the first turn, we read the board a cell at a time, finding safe cells based on the number of adjacent unrevealed cells and the number of mines adjacent to that cell. Each turn, if possible, we pick a cell from the safe list. If we can't, we try to find more. If we can't do that either, we resort to probability, which find the cell with the highest ratio of adjacent mines to unrevealed tiles (i.e. a cell with 2 tiles and 7 unknown is a decently safe bet as opposed to a 4 tile, 5 unknown one).

This current algorithm, like I said, is pretty solid with the parameters that were given with the project. If we up the difficulty to something akin to 'official' _Minesweeper_, we don't fare as well. Not horrible. I created a rating system to determine whether or not my algorithm is improving with each tweak. This is equal to: the average score of the *`900`* games (*`10`* trials of *`90`*, 30 small, 30 medium, 30 large) divided by the max possible score (which is equal to the number of tiles - the number of mines * 2 (a win nets you twice the amount of points), for each size, multiplied by 30 each because of the 30 games), multiplied by a difficulty ratio, which is the number of mines divided by the number of cells.

So for instance, if our large map is *`25x25`* with *`131`* mines, or medium is *`16x16`* with *`40`* mines, and our small is *`9x9`* with *`10`* mines, here's how our difficulty and max score equations look:

*`Difficulty = (131/625 + 40/256 + 10/81) / 3 = 0.163

Max Score = (625 - 131) * 30 * 2 + (256 - 40) * 30 * 2 + (81-10) * 30 * 2 = 40,080`*

Thus, if our average score over the 900 games was 13201, our rating would be: *`13201/40080 * 0.163 * 100 = 5.37`*

Confusing? Maybe, but it allows me to consistently gauge the skill of my AI regardless of the difficulty of the parameters from game to game. In fact, this allows me to see if my AI performs better on more difficult parameters than easier parameters, and vice versa. The rating is out of 10, where 10 would be a perfect score on a map of all mines - 1, making the scale exponential: a 5.37 is pretty good if you graph it on a e^x graph and look at the ratio of the two side.

So far, I have created three 'generations' of my brain, with at least one more planned before the deadline Friday. Here's how they have performed so far, with the class parameters and _Minesweeper_ parameters (the legend stays consistent for each column):

![minesweeper-stats]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/minesweeper_stats.png){: style="width: 900px" .align-center}

As you can see, the average rating goes up over the generations, which show that overall the brain is getting better. There are some tradeoffs though. For class, we are strictly looking at most points, which would point to generation 2 and 3 being pretty neck-and-neck. If we were going just off win percentage, generation 3 is the best bet, but if we were going for percent of perfect score, it would again be a split between generation 2 and 3. It's interesting to see how they differ in games won too. For comparison sake, I rewrote the probability calculation and took into account some other factors (like edge and corner cases, remaining mines, etc.) for gen 2. For gen 3, I changed the starting point (from middle to bottom right), as well as tried to guess more intelligently when picking an adjacent tile to the lowest probability found. I was able to increase all metrics in both parameter sets from gen 1 to gen 3: the class parameters saw a *`0.05`* rating increase, a *`5.22%`* win percentage increase, and a *`0.87%`* percent perfect score increase. The _Minesweeper_ parameters saw a *`1.18`*, *`8.67%`*, and *`7.23%`* increase, respectively, which shows that the brain is indeed getting better at higher difficulties, which I think is arguably more impressive.

For generation 4, more goal is to better account for guessing. If you read my last post (of which there are a dozen sources, most of which were also used when writing the brain), you'd know it pays to guess well. I'm going to try my hand in implementing [something akin to this source](https://nothings.org/games/minesweeper/) to further bolster my attempts at guessing correctly more often when it comes to it. I'm hoping if I succeed, my DLL will do pretty well. It seems to be on its way already.

 

But no matter how hard I try, probability is just that, and [you can't solve _Minesweeper._](http://tyskwo.com/2016/09/06/how-to-solve-minesweeper-hint-you-cant/)
