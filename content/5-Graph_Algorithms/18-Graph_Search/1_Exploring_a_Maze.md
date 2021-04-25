+++

title = "1-Exploring a Maze"

+++

### Graph Search

*Graph Search* algorithms allow us get insights about the structural properties of graph.

### Exploring a Maze

One strategy to explore the maze is unroll a ball of string behind us. The string guarantees that we can always find our way out but we want to explore each part of maze. To do this we need a way to mark the places that we have been to. Consider lights at intersection or doors then we use *Tremaux exploration* to explore-maze.

1. If there are no closed doors at the current intersection, go to step 3. Otherwise open any closed door to any passage leading out of the current intersection (and leave it open)
2. if intersection at other end of passage is lighted, try another door at current intersection ( step 1 ) Otherwise, follow the passage to that intersection, unrolling the strings as you go, turn on the light, and go to step 1.
3. If all the doors at current intersection are open check whether you are at start point. Then stop. If not, use string to go back down the passage that brought you to that intersection for the first time, unrolling the string back up as you go, and look for close door.

*Property 18.1* When we use Tremaux maze exploration, we light all lights and open all doors in the maze and end up back where we started.