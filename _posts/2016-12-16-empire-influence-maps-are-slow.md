---
title: "Empire: Influence Maps are Slow."
description: "Sometimes the best strategy is to be open to new strategies."
---

Or maybe I just wasn't implementing them right. With a map size of 40x40, or 1600 tiles, keeping references to multiple influence maps wasn't too bad, but calculating them was grinding. Regardless, I had to scrap the idea of storing multiple maps and having a blend-weight algorithm for truly diverse interactions and behaviors. With that in mind, I refactored the behavior I wanted my influence maps to exhibit into three key points: find and conquer neutral cities as fast as possible, find and attack enemy cities when they are most weakly-defended, and avoid conflict when possible to keep as many units alive.

Towards point one, this was easily achieved by keeping track of what neutral cities we've tried to conquer but haven't done so. I would then find the closest unit to each neutral city to go for another conquer. Rinse and repeat until they're conquered. For point two, this is a similar procedure as to point one, with the addition of finding how many enemies are within a certain radius of the city (for the competition, I used 2 enemies in a 2-wide radius, but this was not tested thoroughly). When the number of enemies fell below the threshold, the closest army was found and would make a bee-line for the city. Point number three was easiest of all (if also incomplete): my armies would never start a conflict; the anti-thesis to Wayne Gretzky's quote, "You miss 100% of the shots you don't take" – "You win 100% of the conflicts you don't start."

I would calculate refreshes of this data every five turns. Honestly, I believe this was my largest weakness during the competition: at times I was 5 turns behind the current board's info. The number-crunching itself wasn't too intensive – there wasn't any visible delay – and this 5 turn refresh was a hold-over from when I was implementing the influence maps.

Overall, my AI performed diligently. There were a few instances where storms occurred at inopportune times, but outside of the monsoon season, my AI was arguably among the most efficient.
