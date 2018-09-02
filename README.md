# 8_Puzzle_Solver_Java
A simple 8 Puzzle Solver in Java capable of solving all combinations of legal 8 Puzzle games using a variety of algorithms as well as starting positions of the player’s choosing. 

## Algorithms Used:
**1. Uniform Cost Search**
Uniform cost search is the general version of breadth-first search where only the cost of each node is considered with the heuristic function h(n) evaluating to 0 at every node. For the 8 Puzzle game, since each operation takes an atomic step, the weight of every edge in the game tree is always 1. In this case, uniform cost search simply equals breadth-first search.
In my program, in order to make all algorithms as general as possible, I implemented uniform cost search simply as A* search with a heuristic value (h_n) of 0.

**2. A\* with Misplaced Tile Heuristic**
This heuristic function returns the number of misplaced tiles of a state. That is to say, it evaluates every tile of every game board to see if it is in the goal-state location. If not, h_n will increment by 1 for every tile. Hence the worst possible h_n for any 8 Puzzle is 9.

This heuristic encourages the program to expand nodes in the queue that have the most number of correctly placed tiles. This in most cases means a desirable state, although it would misevaluate some layouts such as the following example: <br>
[[\*, 2, 3], <br>
[4, 5, 6], <br>
[7, 8, 1]] <br>
In this case, only two tiles are misplaced: the empty tile and the 1 tile. However, this state is actually quite difficult to solve and would take more moves than say the following example: <br>
[[1, 2, 3], <br>
[4, 6, \*], <br>
[7, 5, 8]] <br>
We can determine by just looking that this case requires only 3 slides to solve (left, down, right), but because it has 4 misplaced tiles instead of 2, its heuristic distance is actually higher.


**3. A\* with Manhattan Heuristic**
To solve this shortcoming, we use another heuristic that returns the sum of L1 distances of all tiles to its goal-position. This distance measure makes intuitive sense because tiles can only slide horizontally or vertically. Both previous examples under this heuristic would have an h_n value of 4. This is a much less biased evaluation of the states although it is still not perfect as we can clearly see in the first example if we were to move 1 back to the upper left corner, we would mess up the ordering of other tiles, so the second case should get a much lower value.

**Algorithms Comparison**
In order to compare each algorithm, I decided to use the 5 preset states in the project description along with the following 5 hard states which I made with the help of the n-puzzle-solver (cited in cover page). These states all rank between doable and ohBoy to fill the sudden jump in difficulty of the ohBoy preset, and are themselves ranked in increasing order of difficulty (solution node depth), with 1 being the easiest and 5 being the most difficult. <br>
*Hard 1......Hard2.......Hard3.......Hard4.......Hard5* <br>
[[1, 5, 2],.[[2, 8, 3],.[[1, 3, 5],.[[5, 6, 1],.[[8, 5, 1], <br>
[4, 3, 0],..[1, 5, 6],....[4, 0, 7],..[4, 2, 3],...[2, 6, 7], <br>
[7, 8, 6]]..[0, 4, 7]]..[8, 2, 6]]..[7, 8, 0]]..[0, 4, 3]] <br>
Finally, the following two properties are used to determine the time complexity of each implementation with respect to the increasing difficulty of problems: number of nodes expanded which is directly related to the performance of an algorithm as well as the actual time taken for the algorithm to terminate, in milliseconds.

The following graph shows the number of nodes expanded for all three algorithms for 10 problems. As can be seen, all three algorithms show similar behavior up to the hard1 difficulty, then starting from hard2, number of nodes expanded under uniform cost search began to increase exponentially. A\* search with the Misplaced Tile Heuristic also displayed an exponential trend after hard4 difficulty. However, A\* search with the Manhattan Distance Heuristic displayed a linear trend throughout the increase in problem complexity.

<img src="https://github.com/MrDavidYu/MrDavidYu/8_Puzzle_Solver_Java/tree/master/res/Num_Nodes_Expanded.png" width="500" height="300" />

The Time Taken graph displays a similar behavior with A\* with Misplaced Tiles, outperforming Uniform Cost Search at all levels except for easy. A\* Manhattan on the other hand, remains the fastest and most scalable algorithm. Note that for the highest difficulty, it is infeasible to use the uniform cost search approach because it uses approximately 524 seconds to find a solution and expanded more than 100,000 nodes.

<img src="https://github.com/MrDavidYu/MrDavidYu/8_Puzzle_Solver_Java/tree/master/res/Time_Taken.png" width="500" height="300" />
<img src="https://github.com/MrDavidYu/MrDavidYu/8_Puzzle_Solver_Java/tree/master/res/Max_Q_Size.png" width="500" height="300" />

To account for space complexity, I also measured the maximum size of the queue for all difficulty settings. Again, the three algorithms behaved as we would expect, with uniform cost search quickly reaching an exponential growth after hard1 and Misplaced Tiles reaching take off after hard4 and Manhattan remaining linear throughout.
