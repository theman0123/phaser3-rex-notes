## Introduction

Find moveable area or moving path, chess behavior of Board system.

- Author: Rex
- Behavior of chess

## Source code

Included in [board plugin](board.md#source-code).

## Usage

[Sample code](https://github.com/rexrainbow/phaser3-rex-notes/tree/master/examples/board-pathfinder)

### Install scene plugin

Included in board plugin.

### Create instance

```javascript
var pathFinder = scene.rexBoard.add.pathFinder(chess, {
    // occupiedTest: false,
    // blockerTest: false,

    // ** cost **
    // cost: 1,   // constant cost
    // costCallback: undefined,
    // costCallbackScope: undefined
    // cacheCost: true,

    // pathMode: 10,  // A*
    // weight: 10,   // weight for A* searching mode
})
```

- `occupiedTest` : Set `true` to test if target tile position is occupied or not, in cost function.
- `blockerTest` : Set `true` to test [blocker property](board-chessdata.md#blocker) in cost function.
- Cost function
    - `cost` : A constant cost for each non-blocked tile
    - `costCallback`, `costCallbackScope` :  Get cost via callback
        ```javascript
        function(curTile, preTile, pathFinder) {
            return cost;
        }
        ```
        - Cost of blocker : `pathFinder.BLOCKER`
- `pathMode`
    - Shortest path
        - `'random'`, or `0`
        - `'diagonal'`, or `1`
        - `'straight'`, or `2`
        - `'line'`, or `3`
    - A* path
        - `'A*'`, or `10`
        - `'A*-random'`, or `11`
        - `'A*-line'`, or `12`
- `weight` : Weight parameter for A* searching mode
- `costCache` : Set `false` to get cost every time. It is useful when cost is a function of (current tile, previous tile).

### Set cost function

- Constant cost for each non-blocked tile
    ```javascript
    pathFinder.setCostFunction(cost);
    ```
- Get cost via callback
    ```javascript
    pathFinder.setCostFunction(callback, scope);
    ```

### Set path mode

```javascript
pathFinder.setPathMode(pathMode)
```

- `pathMode`
    - Shortest path
        - `'random'`, or `0`
        - `'diagonal'`, or `1`
        - `'straight'`, or `2`
        - `'line'`, or `3`
    - A* path
        - `'A*'`, or `10`
        - `'A*-random'`, or `11`
        - `'A*-line'`, or `12`

### Find moveable area

```javascript
var tileXYArray = pathFinder.findArea(movingPoints);
// var out = pathFinder.findArea(movingPoints, out);
```

- `tileXYArray` : An array of moveable tile positions `{x,y,pathCost}`

#### Get shortest path to a moveable tile

```javascript
var tileXYArray = pathFinder.getPath(endTileXY);
```

- `endTileXY` : Tile position of moveable area in last result of `pathFinder.findArea()`
- `tileXYArray` : Moving path in an array of tile positions `{x,y,pathCost}`
    - Uses [moveTo behavior](board-moveto.md) to move chess along path.

!!! note "Path mode"
    - Path info of each tile is calculated during `pathFinder.findArea()`

### Find moving path

```javascript
var tileXYArray = pathFinder.findPath(endTileXY);
```

- `endTileXY` : Tile position
- `tileXYArray` : Moving path in an array of tile positions `{x,y,pathCost}`
    - Uses [moveTo behavior](board-moveto.md) to move chess along path.

!!! note "Path mode"
    - Set `pathMode` to A* (`'A*'`, `'A*-random'`, or `'A*-line'`) to speed up calculating.