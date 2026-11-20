# Passing to Score: Shortest Paths and Greedy Choices on a Football Team

Eleven football players, a ball, and one question: **how should the ball travel to give the best chance of a goal?** The answer depends on what "best" means, and the two parts of this project show how the same situation needs two very different algorithms.

## The setup

A CSV file describes the team as a graph. The value on the diagonal is a player's probability (or time) of shooting, and every other value is the probability (or time) of a successful pass from one player to another. An empty diagonal entry means that player cannot shoot.

## Part 1: Maximising the probability of scoring (shortest path)

The probability that a whole chain of passes works is the **product** of the individual probabilities, and we want to *maximise* it. Shortest-path algorithms minimise a *sum*. The bridge between the two is the logarithm:

```
log(p1 · p2 · p3) = log p1 + log p2 + log p3
```

Taking `-log10` of every pass probability turns "maximise a product" into "minimise a sum" with non-negative edge weights, which is exactly what **Dijkstra's algorithm** solves. After running it from the chosen starting player, the result is multiplied by each candidate shooter's own scoring probability, and the best shooter wins. The program prints the probability matrix and the best path.

## Part 2: Scoring as fast as possible (greedy)

Now the data is *time*, and the player with the ball wants to score as soon as possible. The strategy is **greedy**: if the current player can shoot, shoot; otherwise pass to the teammate who can receive the ball fastest, never to someone who already had it, with ties broken by the lowest index. The program prints the matrix and the path the ball takes.

## Comparing the two

| | Part 1 | Part 2 |
|---|---|---|
| Goal | Maximise probability | Minimise time |
| Method | Dijkstra on `-log` weights | Greedy next step |
| Guarantee | **Optimal** path | Fast, but **not necessarily optimal** |

The greedy rule is simple and instant, but choosing the quickest *next* pass can lead the ball into a dead end that is slower overall. Dijkstra looks at the whole graph and pays for it with a little more work. That is the classic trade-off between greedy heuristics and graph algorithms.

## What you can take from this

1. A change of variables (logarithms) can turn a problem you cannot solve directly into one a standard algorithm solves.
2. Greedy algorithms are only optimal for problems with the right structure; this one does not have it.
3. Check whether the input data (here probabilities versus times) fits the algorithm's assumptions before applying it.

## Files

- `shortest_path.cc`: graph construction, log-transform and Dijkstra
- `greedy_path.cc`: the greedy path
- `probabilities.csv`, `times.csv`: the team data
