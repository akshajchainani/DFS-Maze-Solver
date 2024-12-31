
```markdown
# Depth-First Search (DFS) Implementation

## Overview
This repository contains Python implementations of the Depth-First Search (DFS) algorithm using the `pyamaze` library. The DFS algorithm is used to navigate and find paths in a maze.

### Contents
- **DFS.py**: Basic DFS implementation for solving a maze.
- **DFSDemo.py**: Advanced DFS demonstration with visualization and multiple agents.
- **dfs.csv**: Sample data related to DFS (details may vary).

## Features
- **Maze Creation**: Generate customizable mazes.
- **Pathfinding**: Solve the maze using DFS.
- **Visualization**: Visualize paths with agents in the maze.

## Requirements
- Python 3.x
- Libraries:
  - `pyamaze`

Install the required library with:
```bash
pip install pyamaze
```

## Usage

### Running `DFS.py`
This script demonstrates a simple DFS implementation to find a path through a maze. It generates a maze and visualizes the solved path.

```bash
python DFS.py
```

#### `DFS.py` Code
```python
from pyMaze import maze, agent, COLOR
def DFS(m):
    start = (m.rows, m.cols)
    explored = [start]
    frontier = [start]
    dfsPath = {}
    while len(frontier) > 0:
        currCell = frontier.pop()
        if currCell == (1, 1):
            break
        for d in 'ESNW':
            if m.maze_map[currCell][d] == True:
                if d == 'E':
                    childCell = (currCell[0], currCell[1] + 1)
                elif d == 'W':
                    childCell = (currCell[0], currCell[1] - 1)
                elif d == 'S':
                    childCell = (currCell[0] + 1, currCell[1])
                elif d == 'N':
                    childCell = (currCell[0] - 1, currCell[1])
                if childCell in explored:
                    continue
                explored.append(childCell)
                frontier.append(childCell)
                dfsPath[childCell] = currCell
    fwdPath = {}
    cell = (1, 1)
    while cell != start:
        fwdPath[dfsPath[cell]] = cell
        cell = dfsPath[cell]
    return fwdPath

if __name__ == '__main__':
    m = maze(15, 10)
    m.CreateMaze(loopPercent=100)
    path = DFS(m)
    a = agent(m, footprints=True)
    m.tracePath({a: path})
    m.run()
```

### Running `DFSDemo.py`
This script extends the basic implementation to include advanced features like multiple agents and path visualization.

```bash
python DFSDemo.py
```

#### `DFSDemo.py` Code
```python
from pyamaze import maze, agent, textLabel, COLOR

def DFS(m, start=None):
    if start is None:
        start = (m.rows, m.cols)
    explored = [start]
    frontier = [start]
    dfsPath = {}
    dSearch = []
    while len(frontier) > 0:
        currCell = frontier.pop()
        dSearch.append(currCell)
        if currCell == m._goal:
            break
        poss = 0
        for d in 'ESNW':
            if m.maze_map[currCell][d] == True:
                if d == 'E':
                    child = (currCell[0], currCell[1] + 1)
                if d == 'W':
                    child = (currCell[0], currCell[1] - 1)
                if d == 'N':
                    child = (currCell[0] - 1, currCell[1])
                if d == 'S':
                    child = (currCell[0] + 1, currCell[1])
                if child in explored:
                    continue
                poss += 1
                explored.append(child)
                frontier.append(child)
                dfsPath[child] = currCell
        if poss > 1:
            m.markCells.append(currCell)
    fwdPath = {}
    cell = m._goal
    while cell != start:
        fwdPath[dfsPath[cell]] = cell
        cell = dfsPath[cell]
    return dSearch, dfsPath, fwdPath

if __name__ == '__main__':
    m = maze(10, 10) 
    m.CreateMaze(2, 4) 
    dSearch, dfsPath, fwdPath = DFS(m, (5, 1)) 
    a = agent(m, 5, 1, goal=(2, 4), footprints=True, shape='square', color=COLOR.green)
    b = agent(m, 2, 4, goal=(5, 1), footprints=True, filled=True)
    c = agent(m, 5, 1, footprints=True, color=COLOR.yellow)
    m.tracePath({a: dSearch}, showMarked=True)
    m.tracePath({b: dfsPath})
    m.tracePath({c: fwdPath})
    m.run()
```

### `dfs.csv`
This file contains additional data related to DFS, which can be used for further analysis or logging.


## Author
- [Akshaj chainani]

## Acknowledgments
- Special thanks to the `pyamaze` library for maze generation and visualization.
```

