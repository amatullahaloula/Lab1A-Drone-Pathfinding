# Lab 1 - Search Algorithms for Drone Pathfinding

**Name:** Mohamet Aloula Naima
**Course:** CS254 (Introduction to Artificial Intelligence)
**Lab:** Lab 1 Part A and Part B

## Lab Description

In this lab I am helping a drone find its way through a grid map from a start point to a goal point, avoiding obstacles (and in Part B, avoiding expensive terrain too). The grid is just like a map made of small squares, some squares are blocked (walls) and the drone can only move UP, DOWN, LEFT or RIGHT.

Part A is about uninformed search, which means the drone does not know which direction is closer to the goal, it just explores blindly. Part B is about informed search, where the drone uses a heuristic (a smart guess) to head toward the goal faster.

## Files in this lab

- `Mohamet_Aloula_Naima_lab1A.ipynb` - Part A notebook (BFS, DFS, DLS, IDS)
- `Aloula_Lab_1B.ipynb` - Part B notebook (Greedy, A*, UCS, Weighted A*, IDA*)
- `AI_Use_Declaration_Form.docx` - my filled AI use declaration for this lab

## Part A - Uninformed Search

I built a `Problem` class first, this is just a blueprint that says what every search problem needs: a starting state, a way to check if you reached the goal, a way to get the possible actions, and a way to find the cost of moving.

Then I made `GridProblem` which is the actual drone map. The drone can move UP/DOWN/LEFT/RIGHT and it just checks if the next cell is inside the map and not a wall.

After that I implemented the algorithms:

- **BFS (Breadth-First Search)** - uses a queue (FIFO), explores level by level. Always finds the shortest path because it checks all 1-step neighbors first, then 2-step, then 3-step, etc.
- **DFS (Depth-First Search)** - uses a stack, goes as deep as possible first before backtracking. It can find a path but not always the shortest one.
- **DLS (Depth-Limited Search)** - same as DFS but it stops once it reaches a certain depth. If it didn't find the goal yet it returns "cutoff" instead of "failure", because maybe the answer is just deeper than the limit.
- **IDS (Iterative Deepening Search)** - keeps running DLS again and again with a bigger limit each time (0, 1, 2, 3...) until it finds the goal. This way it gets BFS's guarantee of shortest path but uses less memory like DFS.

I also made two of my own custom maps:
1. A long corridor map to see how BFS and DFS behave differently
2. A map full of dead ends to trap DFS and see how it struggles

## Part B - Informed (Heuristic) Search

This part reuses the same `Problem` and `Node` setup from Part A but adds heuristics, which are basically smart estimates of how far the drone still is from the goal.

I made two heuristic functions:
- **Manhattan distance** - counts steps in a straight grid line (up/down + left/right), ignores walls
- **Euclidean distance** - straight line distance (like a ruler), using Pythagoras theorem

Then I implemented:

- **Greedy Best-First Search** - only looks at the heuristic `h(n)`, picks whatever looks closest to the goal. It's fast but it can get tricked, since it doesn't care how much it already spent getting there.
- **A\* Search** - looks at both the cost so far `g(n)` and the heuristic `h(n)`, so `f(n) = g(n) + h(n)`. This is optimal as long as the heuristic never overestimates the real cost (admissible).
- **Uniform-Cost Search (UCS)** - this is just A* but with `h(n) = 0`, so it only cares about cost so far. Always optimal but it explores a lot more nodes since it has no sense of direction.
- **Weighted A\*** - same as A* but multiplies the heuristic by a weight, to push the search faster toward the goal even if it's not perfectly optimal anymore.
- **IDA\* (Iterative Deepening A\*)** - same idea as IDS from Part A but instead of a depth limit it uses an f-cost limit. This saves memory compared to normal A*.

### Experiments

- **Turbulence map** - I made a zone with very high terrain cost (like flying through bad weather). Greedy goes straight through it because it only cares about getting closer to the goal, but A* goes around it because it actually cares about the total cost.
- **Custom Map 1 (U-shaped trap)** - I built a U-shaped wall that opens away from the goal. Greedy gets lured into the dead end inside the U because the cells look closer to the goal, while A* avoids wasting time there because it also considers the cost paid so far.
- **Custom Map 2 (cheap long way vs expensive short way)** - a 15x15 map where going straight through the middle costs a lot, but going around is longer in steps but actually cheaper overall. This shows that shortest path is not always the cheapest path.
- **Breaking admissibility** - I tried multiplying the heuristic by 3 (making it overestimate). A* with this inflated heuristic is faster but it's not guaranteed to find the optimal path anymore, because the heuristic is lying about the real distance.

## How to run the files

1. Open the notebook in Jupyter Notebook, JupyterLab, or Google Colab.
2. Run all the cells from top to bottom.
3. Each algorithm prints a results table (nodes expanded, path cost, etc.) and some cells plot the actual path on the grid map so you can see it visually.

## Prerequisites

- Python 3
- numpy
- pandas
- matplotlib
