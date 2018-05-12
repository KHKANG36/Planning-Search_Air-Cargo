
# Implement a Planning Search

## Synopsis
 
In this project, I implemented algorithms which solve deterministic logistics planning problems for an Air Cargo transport system using a planning search agent. This project is one of the Udacity AI nanodegree program. 

## Instructions

You need to configure the AIND environment
 - Setup the [Anaconda](https://www.continuum.io/downloads) environment.
 - Setup the AIND environment : Download the `aind-universal.yml` file in this repository. Open a terminal and run `conda env create -f aind-universal.yml` to create the environment.

## Start Guide

### Activate the aind environment (OS X or Unix/Linux)
    
    `$ source activate aind`

### Activate the aind environment (Windows)

    `> activate aind`

### Run the code & visualization

    `(aind)$ python run-search.py -m`

## Project Implementation

### Project Goal : classical PDDL problems

All problems are in the Air Cargo domain.  They have the same action schema defined, but different initial states and goals.

- Air Cargo Action Schema:
```
Action(Load(c, p, a),
	PRECOND: At(c, a) ∧ At(p, a) ∧ Cargo(c) ∧ Plane(p) ∧ Airport(a)
	EFFECT: ¬ At(c, a) ∧ In(c, p))
Action(Unload(c, p, a),
	PRECOND: In(c, p) ∧ At(p, a) ∧ Cargo(c) ∧ Plane(p) ∧ Airport(a)
	EFFECT: At(c, a) ∧ ¬ In(c, p))
Action(Fly(p, from, to),
	PRECOND: At(p, from) ∧ Plane(p) ∧ Airport(from) ∧ Airport(to)
	EFFECT: ¬ At(p, from) ∧ At(p, to))
```

- Problem 1 initial state and goal:
```
Init(At(C1, SFO) ∧ At(C2, JFK) 
	∧ At(P1, SFO) ∧ At(P2, JFK) 
	∧ Cargo(C1) ∧ Cargo(C2) 
	∧ Plane(P1) ∧ Plane(P2)
	∧ Airport(JFK) ∧ Airport(SFO))
Goal(At(C1, JFK) ∧ At(C2, SFO))
```
- Problem 2 initial state and goal:
```
Init(At(C1, SFO) ∧ At(C2, JFK) ∧ At(C3, ATL) 
	∧ At(P1, SFO) ∧ At(P2, JFK) ∧ At(P3, ATL) 
	∧ Cargo(C1) ∧ Cargo(C2) ∧ Cargo(C3)
	∧ Plane(P1) ∧ Plane(P2) ∧ Plane(P3)
	∧ Airport(JFK) ∧ Airport(SFO) ∧ Airport(ATL))
Goal(At(C1, JFK) ∧ At(C2, SFO) ∧ At(C3, SFO))
```
- Problem 3 initial state and goal:
```
Init(At(C1, SFO) ∧ At(C2, JFK) ∧ At(C3, ATL) ∧ At(C4, ORD) 
	∧ At(P1, SFO) ∧ At(P2, JFK) 
	∧ Cargo(C1) ∧ Cargo(C2) ∧ Cargo(C3) ∧ Cargo(C4)
	∧ Plane(P1) ∧ Plane(P2)
	∧ Airport(JFK) ∧ Airport(SFO) ∧ Airport(ATL) ∧ Airport(ORD))
Goal(At(C1, JFK) ∧ At(C3, JFK) ∧ At(C2, SFO) ∧ At(C4, SFO))
```

### Project Implementation Result

In this project, I implemented the 3 air-cargo planning logic which moves cargos from airport to airport through the airplane, After that, I found the best search algorithm to resolve the air-cargo problems via several tests with a couple of search algorithms. I used ‘breadth first search’, ‘depth first graph search’ and ‘uniform cost search’ for non-heuristic search and ‘A* planning search’ with ‘ignore precondition’ and ‘level-sum’ heuristic for heuristic search. 

#### 1. Uninformed (Non-heuristic) Planning search result for 3 air-cargo problems. 
1) Breadth first search 
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Breadth.jpg)

2) Depth first graph search 
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Depth.jpg)

3) Uniform cost search 
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Uniform.jpg)

#### 2. A* (Heuristic) planning search result for 3 air-cargo problems. 
1) A* search 
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Astar.jpg)

2) A* search with ignore precondition heuristic
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Astar_precond.jpg)

3) A* search with level-sum heuristic
![Test image](https://github.com/KHKANG36/Planning-Search_Air-Cargo/blob/master/images/Astar_levelsum.jpg)

#### 3. Analysis of the result
First of all, in terms of performance on non-heuristic search, breadth fist search and uniform cost search (a sort of improved search algorithm of breadth first search) find the optimized path length. Otherwise, depth first graph search shows better result on searching time, the number of expansion, goal test, new nodes. This is basically caused by the chracteristic of each search algorithm. Breadth first search algorithm explores the entire frontiers of all subtrees, and the search time and expansion is pretty huge. However, it is able to find the optimized(shortest) path. Depth first graph search is able to arrive the goal faster with small number of node explansion, however the path should not be optimal. In this tests, uniform cost search algorithm did not show the better performance than that of breadth first search even though it requires the more memory and calculation. 

Secondly, in terms of performance on A* heuristic search, all of three was able to optimized the path. A* search with ignore precondition showed the best searching time. Otherwise, A* search with level-sum provides the smallest expansion, goal test and new nodes. Because A* search with level-sum continuously provides information on the expected level-sum toward the goal, it can minimize the expansion and goal test. 

If we discuss the best solution for these problems, obviously, the heuristic search planning methods are better than non-heuristic search planning methods. In my opinion, A* search with ignore precodition heuristic is the best heuristic search because I think that optimized path length and the search time is the most important factor for these problems. Even though A* seach with level-sum heuristic shows the best performance in expansion and goal test, the search time increased dramatically when the problem is getting complex.  
