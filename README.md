# Avoid Obstacles: Balanced AI

A real-time desktop game built with Python and Tkinter. You control a blue square with the arrow keys and must avoid red obstacles that chase you using **Dijkstra's shortest-path algorithm**. The obstacles also move randomly some of the time, and they get faster as the game goes on.

## How it works

- **Grid world:** the 600×400 canvas is a grid of 20-pixel cells (30 columns × 20 rows). The player and every obstacle occupy one cell and move one cell at a time.
- **Chasing with Dijkstra:** about 85% of the time, each obstacle computes the shortest path to the player with Dijkstra's algorithm (a `heapq` priority queue, a visited set, and a came-from map to rebuild the path) and steps along it.
- **Controlled randomness:** the other 15% of the time (`random_move_chance = 0.15`), the obstacle takes a random step. Without it the AI is too predictable and too hard to escape. In testing, a higher chance made it too easy and a lower chance made it nearly unbeatable.
- **Rising difficulty:** the delay between obstacle moves starts at 300 ms and drops by 10 ms every 10 obstacle moves, down to a minimum of 150 ms.
- **Collision:** the game ends when the player's box overlaps an obstacle's box, and "Game Over" is shown.

## Controls

| Key | Action |
| --- | --- |
| Arrow keys | Move the blue square one cell |

Goal: survive as long as possible. The game ends when an obstacle touches you.

## Requirements

- Python 3 with Tkinter (included in the standard Windows installer from python.org)
- No extra packages. The game uses only `tkinter`, `heapq` and `random`.

## Run

```bash
python main.py
```

## Settings you can change

These are set at the top of the `AvoidObstaclesGameDijkstra` class in `main.py`:

| Setting | Default | Meaning |
| --- | --- | --- |
| `num_obstacles` | 3 | Number of red obstacles (3 to 5 stays smooth in Tkinter) |
| `random_move_chance` | 0.15 | Chance that an obstacle moves randomly |
| `ai_delay` | 300 | Starting delay between obstacle moves, in ms |
| `difficulty_increment` | 10 | Delay reduction every 10 obstacle moves, in ms |

## Results from testing

- The shortest path was found correctly with the player standing still, moving randomly, trying to dodge, and with several obstacles converging.
- Randomness at 15% gave the most balanced play.
- The speed-up worked as intended, and Tkinter handled it without breaking the animation.
- Players described the game as calm at first, tense in the middle, and a survival challenge at the end.

## Problems solved during development

| Problem | Solution |
| --- | --- |
| Obstacles became repetitive and too predictable | Added the 15% random move chance |
| Obstacles became unbeatable when they sped up too fast | Set a minimum delay of 150 ms |
| Obstacles "jumped" when a path could not be rebuilt | Added checks that the path is complete before using it |
| Random moves pushed obstacles off the grid | Added boundary checks before moving |
| Unexpected collisions at speed | Kept all movement aligned to grid cells |
| Laggy animation with many obstacles | Limited the number of obstacles |

## Limitations and ideas

- The game has no restart button or score display. Close the window and run it again to play again.
- The grid is small, so full Dijkstra recalculation is cheap. On larger maps, A* would be a better choice.
- The player can currently move past the edge of the canvas, because boundary checks apply only to the obstacles.
- Possible additions: multiple levels and grid sizes, power-ups, an AI that predicts the player's movement, sound and animations, and a multiplayer mode.

## Tech

Python, Tkinter, `heapq` (priority queue), Dijkstra's algorithm