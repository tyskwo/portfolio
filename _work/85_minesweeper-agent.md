---
title: "Minesweeper Agent"
excerpt: "An AI brain that tries to win Minesweeper."

header:
  image: /assets/images/portfolio/2016/minesweeper/feature.png
  teaser: /assets/images/portfolio/2016/minesweeper/feature.png

language: "C++"
year: "2016"
portfolio: "true"

sidebar:
  - text: "## 2016, C++ ##"


  - title: "Description"

    text: "An AI brain that tries to win the un-winnable Minesweeper."


  - title: "Outcomes"

    text: "Linking with a DLL.


    Set mathematics."


  - title: "Links"

  - button: Repository
    link: https://github.com/tyskwo/MinesweeperAgent
    icon: github
---

_Minesweeper Agent_ is an AI project which tries to solve Minesweeper boards. It implements much of the content on [this page](http://www.minesweeper.info/wiki/Strategy).

Blog posts relating to this project can be found [here]({{ site.url }}{{ site.baseurl }}/2016/how-to-solve-minesweeper/), [here]({{ site.url }}{{ site.baseurl }}/2016/minesweeper-update/), and [here]({{ site.url }}{{ site.baseurl }}/2016/minesweeper-the-final-act/).


### Architecture

This was originally a project for my _Artificial Opponents_ class: a course where we'd try to create AI for multiple different games (such as [gin rummy]({{ site.url }}{{ site.baseurl }}/2016/gin-rummy-the-conclusion/)). The structure of the project was broken down into the game, and a DLL containing our AI. The game was written by the professor, and it called methods through the DLL at regular intervals. For _Minesweeper_, this was when we needed to pick a tile. At that point, our AI would return a tile index to choose, and the game would go back and execute the relevant logic.

![minesweeper]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2016/minesweeper/example1.gif){: .align-center}

### AI Brain

I created an AI 'brain' which would keep track of what tiles are revealed/hidden, and find tiles that are safe based on the number of mines at a given revealed index and how many unrevealed adjacent tiles there were:

{% highlight cpp %}
Tile MineBrain::FindSafeTile()
{
  //update our knowledge of what's revealed/unrevealed
  revealed   = GetAllRevealedTiles();
  unrevealed = GetAllUnrevealedTiles();


  for (Tile atIndex : revealed)
  {
    //check to see the number of flagged tiles vs the number of bombs
    CheckTileForMines(atIndex);

    //if we found all of the bombs in the level, add all unrevealed, unflagged tiles to the safe list
    if (bombs.size() == numberOfMines) return AddRestToSafe();
  }

  //find the safest tile to click
  return CheckAllRevealedProbabilities();
}

-----

void MineBrain::CheckTileForMines(Tile index)
{
  //get the number of bombs at the give tile
  Number minesAtIndex = board->getNumAdjacentMines(index);

  //if there's no bombs, exit early
  if (minesAtIndex == 0) return;


  //get the adjacent tiles
  std::vector<Tile> adjacentTiles = GetUnrevealedAdjacencies(index);

  //if the number of adjacent tiles is equal to the number of mines...
  if (adjacentTiles.size() == minesAtIndex)
  {
    //flag all of the adjacent tiles (if they're not already flagged)
    for (Tile index : adjacentTiles)
    {
      if (std::find(bombs.begin(), bombs.end(), index) == bombs.end()) bombs.push_back(index);
    }
  }
}
{% endhighlight %}

These safe tiles would then be added to a list to be pulled from when the game asks us to return a tile to pick.

![minesweeper]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2016/minesweeper/example2.gif){: .align-center}


### Retrospection and Postmortem

There was one pattern that I wasn't able to implement: the 1-2 pattern (and it's sister, the 1-2-1). These patterns take into account multiple tiles in multiple orientations to deduce whether an unrevealed tile is safe or a bomb. Other than that, I think my AI was well organized, and performed well in the class competition (see [this blog post]({{ site.url }}{{ site.baseurl }}/2016/minesweeper-the-final-act/) for the results of that).

Also, throughout the iteration of this project, I ran tests to see the general aptitude of my AI, and I'm happy to report that as I changed weightings of certain algorithms and added more safety checks for determining bomb tiles, it saw a marked improvement:

<figure>
    <img src="/assets/images/portfolio/2016/minesweeper/final-results.png">
    <figcaption>From the first generation of the 'brain' to the last.</figcaption>
</figure>
