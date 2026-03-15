Methods:

Mostly using the browser editor, after implementing most of my solution I tried out Claude code for the first time for this, I was impressed and was able to test out a lot of ideas but for optimization (and some accuracy) reasons ended up re-writing most things it wrote by hand.

The biggest timesink on problems was spent staring at the ungathered food in the gauntlet and fortress maps thinking about how to extract it.

Strategy:

Maintain pheromone gradients for both food sources and nest:
  Direct reckoning seemed expensive when so many maps had a lot of walls, so ditch that and stick to pheromones only.
  
  Value of current cell will always be the value of the nearest neighbor - 1, any ant can "extend" the trail of an adjacent pheromone gradient to the current location.

Alternating decrements:
  When updating a trail, on alternating cells (checkerboard pattern), set the value to be equal to the nearest neighbor instead of -1. Depending on whether or not an ant is on a black/white grid, the trail direction will either be equal or greater than the current cell.
  This is to "extend" the length of a trail, otherwise they can decay too quickly for long maps.

  Max trail length (no decrementing): 255
  Max trail length (decrement every cell): 127
  Max trail length (decrement alternating): in between

Pheromone extension trail:
  Dedicate a pheromone to extending the trails leading to both food and the nest. Probably is the more effective "extension" method than the above. Really helpful for fortress/gauntlet maps.

Ignore trails:
  If reaching the end of a trail and finding nothing, roam ignoring trails for 15ish ticks, to prevent going back/forth in one spot until the trail disappears.

Search pattern:
  Simple random walking doesn't explore the map very efficiently, instead search with a direction, and randomly turn around once every 25 ticks.

  Also do wall following. This helps with both the brush/maze maps, but also seems to help with the maps with chambers/curved portions, where ants can bounce around a lot without wall following.
