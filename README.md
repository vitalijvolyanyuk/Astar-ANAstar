# Grid-Based Path Planning

> Note: This was a final project. To respect course policies, I am not publishing the code. This repository contains a high-level summary and key takeaways.

## Overview

I implemented A* and Anytime Nonparametric A* (ANA*) [1] for a holonomic mobile robot in a discretized SE(2) grid. The project examines how the planners behave in challenging environments, including occlusion, narrow-passage, and dead-end scenarios, and how different heuristic choices affect their performance in each.

## Motivation
Informed algorithms such as A* are often used for navigation tasks because they can guarantee optimal solutions when an admissible heuristic is used. However, designing a heuristic that remains informative in complex environments is difficult. In environments where obstacles block direct paths to the goal or trap the robot, heuristics can be misleading, causing larger regions of the space to be searched in order to guarantee optimality. This results in significant computation time and can be limiting for navigation tasks under time constraints. In real-world scenarios where a robot must act urgently, such as hospital environments, waiting for optimality guarantees is often not practical.

ANA*, an anytime variant of A*, addresses this problem by finding a feasible solution quickly and iteratively improving it over time. While ANA* still converges to an optimal solution given an admissible heuristic and sufficient time, its anytime behavior allows potentially useful solutions to be found earlier. ANA* finds a feasible solution quickly because its initial search is driven by the heuristic alone rather than by total cost estimates like A*. This makes the quality of early solutions more sensitive to the environment and heuristic choice, which motivates comparing A* and ANA*.

## Search-based Planners

Search-based planners discretize a robot's state space into a graph and search it for a lowest-cost collision-free path. Nodes represent states, and edges represent actions with associated costs. To decide which node to expand next, both searches use the same two costs:

- g(n): the accumulated cost from the start to node n (cost-to-come)
- h(n): the heuristic estimate of the remaining cost from n to the goal (cost-to-go)

A* and ANA* combine these differently, which results in different search behavior.

## A*

A* maintains a priority queue of nodes ordered by the evaluation
function f(n) = g(n) + h(n). At each iteration it pops and expands the lowest f(n),
generates its successors, and inserts them into the queue. Given an
admissible heuristic, which never overestimates the true cost-to-go,
A* returns an optimal path. Euclidean distance is a common choice, since
a straight line is never longer than any obstacle-free path.

To guarantee optimality, the search does not terminate when the goal is generated as a successor and inserted into the queue. It terminates when the goal node is popped and expanded, which means every node with f(n) < C* must be expanded first, where C* is the optimal path cost. An uninformed heuristic that underestimates the remaining cost results in many nodes with similar costs less than C*. This makes the optimality guarantee expensive when using a pure Euclidean heuristic in the presence of obstacles blocking direct paths to the goal.

## ANA*




## References
[1] J. van den Berg, R. Shah, A. Huang, and K. Goldberg, "ANA*: Anytime Nonparametric A*," 2011.
