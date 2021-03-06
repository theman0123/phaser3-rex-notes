## Introduction

Layout children game objects.

It is inspired from [wxSizer](https://docs.wxwidgets.org/3.0/overview_sizer.html).

- Author: Rex
- Game object

## Source code

[Plugin](https://github.com/rexrainbow/phaser3-rex-notes/blob/master/templates/ui/ui-plugin.js), [minify](https://github.com/rexrainbow/phaser3-rex-notes/blob/master/plugins/dist/rexuiplugin.min.js)

[Class](https://github.com/rexrainbow/phaser3-rex-notes/blob/master/templates/ui/sizer/Sizer.js)

## Usage

[Sample code](https://github.com/rexrainbow/phaser3-rex-notes/tree/master/examples/ui-sizer)

### Install plugin

Install plugin in [configuration of game](game.md#configuration)

```javascript
import UIPlugin from 'rexTemplates/ui/ui-plugin.js';

var config = {
    // ...
    plugins: {
        scene: [{
            key: 'rexUI',
            plugin: UIPlugin,
            mapping: 'rexUI'
        },
        // ...
        ]
    }
    // ...
};
var game = new Phaser.Game(config);
```

### Add sizer object

```javascript
var sizer = scene.rexUI.add.sizer({
    orientation: 0,
    // x: 0,
    // y: 0,
    // width: undefined,
    // height: undefined
});
```

or

```javascript
var sizer = scene.rexUI.add.sizer(x, y, {
    orientation: 0,
    // width: undefined,
    // height: undefined
});
```

or

```javascript
var sizer = scene.rexUI.add.sizer(x, y, width, height, {
    orientation: 0,
});
```

or

```javascript
var sizer = scene.rexUI.add.sizer(x, y, width, height, orientation);
```

- `orientation` : Main orientation of the sizer.
    - `'left-to-right'`, `'horizontal'`,`'h'`, `'x'`, or `0` : Arrange game objects from left ot right.
    - `'top-to-bottom'`, `'vertical'`,`'v'`, `'y'`, or `1` : Arrange game objects from top to bottom.
- `x`, `y` : Position of sizer. Only available for top-sizer, children-sizers will be changed by parent.
- `width` : Minimum width. i.e. Width of this sizer will larger then this value.
- `height` : Minimum height. i.e. Hieght of this sizer will larger then this value.

### Custom class

- Define class
    ```javascript
    class MySizer extends RexPlugins.UI.Sizer {
        constructor(scene, x, y, minWidth, minHeight, orientation) {
            super(scene, x, y, minWidth, minHeight, orientation);
            // ...
        }
        // ...
    }
    ```
- Create instance
    ```javascript
    var sizer = new MySizer(scene, x, y, minWidth, minHeight, orientation);
    ```

### Add child

Add a game obejct to sizer

```javascript
sizer.add(child);
```

or

```javascript
sizer.add(child, proportion, align, paddingConfig, expand);
```

- `child` : A game object
- `proportion` :
    - `0`, or `'min'` : Place next game object closely. Default value.
    - `> 0` :
    - `-1`, or `'full'` : Stretch game object in the main orientation of the sizer.
    - `null` : Don't arrange this child
- `align` :
    - `'center'`, or `Phaser.Display.Align.CENTER` : Align game object at center. Default value.
    - `'left'`, or `Phaser.Display.Align.LEFT_CENTER` : Align game object at left-center.
    - `'right'`, or `Phaser.Display.Align.RIGHT_CENTER` : Align game object at right-center.
    - `'top'`, or `Phaser.Display.Align.RIGHT_CENTER` : Align game object at top-center.
    - `'bottom'`, or `Phaser.Display.Align.BOTTOM_CENTER` : Align game object at bottom-center.
- `paddingConfig` : Add space between bounds. Default is 0.
    - A number for left/right/top/bottom bounds
    - Or a plain object
        ```javascript
        {
            left: 0,
            right: 0,
            top: 0,
            bottom: 0
        }
        ```
- `expand` : Set `true` to
    - Expand height when `orientation` is `0` (`left-to-right`), or
    - Expand width when `orientation` is `1` (`top-to-bottom`)

### Add background

```javascript
sizer.addBackground(child, paddingConfig);
```

### Layout children

Arrange position of all children.

```javascript
sizer.layout();
```

### Bounds of sizer

- Get
    ```javascript
    var leftBound = sizer.left;
    var rightBound = sizer.right;
    var topBound = sizer.top;
    var bottomBound = sizer.bottom;
    ```
- Set
    ```javascript
    sizer.left = leftBound;
    sizer.right = rightBound;
    sizer.top = topBound;
    sizer.bottom = bottomBound;
    ```
    or
    ```javascript
    sizer.alignLeft(leftBound);
    sizer.alignRight(rightBound);
    sizer.alignTop(topBound);
    sizer.alignBottom(bottomBound);
    ```

### Push into bounds

Align sizer to bound if overlapping it.

```javascript
sizer.pushIntoBounds();
```

or

```javascript
sizer.pushIntoBounds(bounds);
```

- `bounds` : Bounds in [rectangle object](geom-rectangle.md).

### Scale

- Pop up
    - Pop up width and height
        ```javascript
        sizer.popUp(duration);
        // sizer.popUp(duration, undefined, ease);
        ```
        - `ease` : [Ease function](tween.md/#ease-equations), default is `'Cubic'`.
    - Pop up width only
        ```javascript
        sizer.popUp(duration, 'x');
        // sizer.popUp(duration, 'x', ease);
        ```
        - `ease` : [Ease function](tween.md/#ease-equations), default is `'Cubic'`.
    - Pop up height only
        ```javascript
        sizer.popUp(duration, 'y');
        // sizer.popUp(duration, 'y', ease);
        ```
        - `ease` : [Ease function](tween.md/#ease-equations), default is `'Cubic'`.
- Scale down destroy
    - Scale down width and height
        ```javascript
        sizer.scaleDownDestroy(duration);
        ```
    - Scale down width only
        ```javascript
        sizer.scaleDownDestroy(duration, 'x');
        ```
    - Scale down height only
        ```javascript
        sizer.scaleDownDestroy(duration, 'y');
        ```

### Fade

- Fade in
    ```javascript
    sizer.fadeIn(duration);
    ```
- Fade out destroy
    ```javascript
    sizer.fadeOutDestroy(duration);
    ```

### Draw bounds

Draw all bounds of children.

```javascript
sizer.drawBounds(graphics, color);
```

- `graphics` : [Graphics game object](graphics.md)
- `color` : Default value is `0xffffff`

### Other properties

This sizer game object inherits from [ContainerLite](containerlite.md).