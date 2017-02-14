---
title: "Minesweeper: The Final Act"
description: "In the epic conclusion to Minesweeper, we finally find out just how well I did."
---

(See [here](http://tyskwo.com/2016/09/06/how-to-solve-minesweeper-hint-you-cant/) for the planning post, and [here](http://tyskwo.com/2016/09/12/minesweeper-update/) for the midpoint check-in)

The deadline has now passed. I was able to create a fourth generation, which has a much better weighting algorithm. The 'holy grail' fifth generation with [the 1-2 pattern matching](http://www.minesweeper.info/archive/BrianChuMinesweeper/1-2_lesson/) is in progress, but I wasn't able to implement it in time. If I was able to implement it, the win percentage would increase, as I hopefully would not have to guess as much. Regardless, Gen 3 to Gen 4 saw a decent increase for both the class parameters and official Minesweeper parameters.

![minesweeper-stats]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/minesweeper1.gif){: style="width: 900px" .align-center}

Generation 4 of the brain introduced a more concise weighting algorithm. We still find the tile with the lowest probability of clicking an adjacent mine, but we then take the adjacent tiles that aren't currently flagged, and perform a weighting calculation based on the neighbors to those tile. For instance, if tile A has 3 revealed tiles adjacent to it, and their numbers are 1, 2, and 2, and we have 1 flagged, the weight for A is 4, and we divide it by the number of unrevealed, non-flagged tiles. In this case that is 8-3-2, so 4. The final weight is 1. We do this for each of the neighbors of the lowest probability tile, and pick the lowest weight. The weight can be looked at a probability of sorts, but it is not a percent. Lower weights mean that we know more of the surrounding tiles - it's arguably a safer pick - whereas higher weights mean we know less about the surrounding tiles - it can be a safer pick, but we're not sure.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/minesweeper2.gif" alt="">
  <figcaption>In the beginning, you can see it guess using weighting when moving into the top-left corner.</figcaption>
</figure>

After testing, Gen 4 is slightly better than Gen 3. Over 900 games with class parameters, it achieved a *`93%`* average perfect score, with one round being an impressive *`99.1%`* perfect (45958/46360). On the _Minesweeper_-side, the win percentage jumped almost *`9%`* from Gen 3. Here are the deltas from Gen 1 to Gen 4:

![minesweeper-stats]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/minesweeperResults.png){: style="width: 900px" .align-center}

Overall, a pretty modest improvement. It's hard to judge exactly how much of it comes down to RNG, especially with the games that end after a bad guess on the second turn. I'm more proud of how much better the brain performed with the _Minesweeper_ parameters; a 17% increase in number of games won is pretty substantial. In the class 'competition,' I came in 13th with the normal parameters, but with 8L, 20M, and 20S games. It made a few bad guesses at the beginning of three of the large games which is what put me so far back in the pack - the difference was still only 1300 points between first and me. Running the test on my computer, I achieved a score of 16420, which squeaks me into first. This is only with two sets of data though, so the rankings really don't mean much. With that said, I did place first with the _Minesweeper_ parameters.

Moving forward, I'll see if I can finish Gen 5, and see how well that performs in comparison. Depending on how the rest of the class unfolds, this might end up in my portfolio - I'll wait to see if I do better on any of the other projects. This was extremely fun and satisfying though, and I can't wait to get my hands on _Gin Rummy_. If you're on a Mac, [here's the Numbers file for all of my data crunching](https://www.dropbox.com/s/1ena54zfyoa3g5b/Minesweeper.numbers?dl=0), feel free to come up with better equations for comparing the generations: I am by no means a statistician. I'll zip up the source code soon.
