# icp-demo-projects-2

# Ball Game


`index.html`

```js

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width,user-scalable=no">
    <meta name="viewport" content="width=device-width" />
    <title>buildBlocks</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  </head>
  <body>
    <p id="score"></p>
    <p id="playername"></p>
      <canvas id="canvas"></canvas>
  </body>
</html>
```

`index.js`

```js

import { buildBlocks_backend } from "../../declarations/buildBlocks_backend";

var a = prompt("Enter your name!");

var b = document.getElementById('playername');
b.textContent = await buildBlocks_backend.enterName(a);

var map = {

  tile_size: 16,

  keys: [
    { id: 0, colour: '#333', solid: 0 },
    { id: 1, colour: '#888', solid: 0 },
    { id: 2, colour: '#555', solid: 1, bounce: 0.35 },
    { id: 3, colour: 'rgba(121, 220, 242, 0.4)', friction: { x: 0.9, y: 0.9 }, gravity: { x: 0, y: 0.1 }, jump: 1, fore: 1 },
    { id: 4, colour: '#777', jump: 1 },
    { id: 5, colour: '#E373FA', solid: 1, bounce: 1.1 },
    { id: 6, colour: '#666', solid: 1, bounce: 0 },
    { id: 7, colour: '#73C6FA', solid: 0, script: 'change_colour' },
    { id: 8, colour: '#FADF73', solid: 0, script: 'next_level' },
    { id: 9, colour: '#C93232', solid: 0, script: 'death' },
    { id: 10, colour: '#555', solid: 1 },
    { id: 11, colour: '#0FF', solid: 0, script: 'unlock' }
  ],

  data: [
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 6, 6, 6, 6, 6, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 2, 1, 1, 1, 1, 1, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 7, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 2, 2, 2, 4, 2, 2, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 8, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 6, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 9, 9, 9, 2, 10, 10, 10, 10, 10, 10, 1, 1, 1, 1, 1, 1, 1, 11, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 10, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 6, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 1, 1, 1, 1, 1, 1, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2],
    [2, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 6, 6, 6, 2, 2, 2, 2, 2, 2, 6, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 1, 1, 1, 1, 2, 5, 5, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 5, 5, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2]
  ],

  gravity: {
    x: 0,
    y: 0.3
  },

  vel_limit: {
    x: 2,
    y: 16
  },

  movement_speed: {
    jump: 6,
    left: 0.3,
    right: 0.3
  },


  player: {
    x: 2,
    y: 2,
    colour: '#FF9900'
  },

  scripts: {
    change_colour: 'game.player.colour = "#"+(Math.random()*0xFFFFFF<<0).toString(16);',
    next_level: 'alert("Yay! You won! Reloading map.");game.load_map(map);',
    death: 'alert("You died!");game.load_map(map);',
    unlock: 'game.current_map.keys[10].solid = 0;game.current_map.keys[10].colour = "#888";'
  }
};

var Clarity = function () {

  this.alert_errors = false;
  this.log_info = true;
  this.tile_size = 16;
  this.limit_viewport = false;
  this.jump_switch = 0;

  this.viewport = {
    x: 200,
    y: 200
  };

  this.camera = {
    x: 0,
    y: 0
  };

  this.key = {
    left: false,
    right: false,
    up: false
  };

  this.player = {

    loc: {
      x: 0,
      y: 0
    },

    vel: {
      x: 0,
      y: 0
    },

    can_jump: true
  };

  window.onkeydown = this.keydown.bind(this);
  window.onkeyup = this.keyup.bind(this);
};

Clarity.prototype.error = function (message) {

  if (this.alert_errors) alert(message);
  if (this.log_info) console.log(message);
};

Clarity.prototype.log = function (message) {

  if (this.log_info) console.log(message);
};

Clarity.prototype.set_viewport = function (x, y) {

  this.viewport.x = x;
  this.viewport.y = y;
};

Clarity.prototype.keydown = function (e) {

  var _this = this;

  switch (e.keyCode) {
    case 37:
      _this.key.left = true;
      break;
    case 38:
      _this.key.up = true;
      break;
    case 39:
      _this.key.right = true;
      break;
  }
};

Clarity.prototype.keyup = function (e) {

  var _this = this;

  switch (e.keyCode) {
    case 37:
      _this.key.left = false;
      break;
    case 38:
      _this.key.up = false;
      break;
    case 39:
      _this.key.right = false;
      break;
  }
};

Clarity.prototype.load_map = function (map) {

  if (typeof map === 'undefined'
    || typeof map.data === 'undefined'
    || typeof map.keys === 'undefined') {

    this.error('Error: Invalid map data!');

    return false;
  }

  this.current_map = map;

  this.current_map.background = map.background || '#333';
  this.current_map.gravity = map.gravity || { x: 0, y: 0.3 };
  this.tile_size = map.tile_size || 16;

  var _this = this;

  this.current_map.width = 0;
  this.current_map.height = 0;

  map.keys.forEach(function (key) {

    map.data.forEach(function (row, y) {

      _this.current_map.height = Math.max(_this.current_map.height, y);

      row.forEach(function (tile, x) {

        _this.current_map.width = Math.max(_this.current_map.width, x);

        if (tile == key.id)
          _this.current_map.data[y][x] = key;
      });
    });
  });

  this.current_map.width_p = this.current_map.width * this.tile_size;
  this.current_map.height_p = this.current_map.height * this.tile_size;

  this.player.loc.x = map.player.x * this.tile_size || 0;
  this.player.loc.y = map.player.y * this.tile_size || 0;
  this.player.colour = map.player.colour || '#000';

  this.key.left = false;
  this.key.up = false;
  this.key.right = false;

  this.camera = {
    x: 0,
    y: 0
  };

  this.player.vel = {
    x: 0,
    y: 0
  };

  this.log('Successfully loaded map data.');

  return true;
};

Clarity.prototype.get_tile = function (x, y) {

  return (this.current_map.data[y] && this.current_map.data[y][x]) ? this.current_map.data[y][x] : 0;
};

Clarity.prototype.draw_tile = function (x, y, tile, context) {

  if (!tile || !tile.colour) return;

  context.fillStyle = tile.colour;
  context.fillRect(
    x,
    y,
    this.tile_size,
    this.tile_size
  );
};

Clarity.prototype.draw_map = function (context, fore) {

  for (var y = 0; y < this.current_map.data.length; y++) {

    for (var x = 0; x < this.current_map.data[y].length; x++) {

      if ((!fore && !this.current_map.data[y][x].fore) || (fore && this.current_map.data[y][x].fore)) {

        var t_x = (x * this.tile_size) - this.camera.x;
        var t_y = (y * this.tile_size) - this.camera.y;

        if (t_x < -this.tile_size
          || t_y < -this.tile_size
          || t_x > this.viewport.x
          || t_y > this.viewport.y) continue;

        this.draw_tile(
          t_x,
          t_y,
          this.current_map.data[y][x],
          context
        );
      }
    }
  }

  if (!fore) this.draw_map(context, true);
};

Clarity.prototype.move_player = function () {

  var tX = this.player.loc.x + this.player.vel.x;
  var tY = this.player.loc.y + this.player.vel.y;

  var offset = Math.round((this.tile_size / 2) - 1);

  var tile = this.get_tile(
    Math.round(this.player.loc.x / this.tile_size),
    Math.round(this.player.loc.y / this.tile_size)
  );

  if (tile.gravity) {

    this.player.vel.x += tile.gravity.x;
    this.player.vel.y += tile.gravity.y;

  } else {

    this.player.vel.x += this.current_map.gravity.x;
    this.player.vel.y += this.current_map.gravity.y;
  }

  if (tile.friction) {

    this.player.vel.x *= tile.friction.x;
    this.player.vel.y *= tile.friction.y;
  }

  var t_y_up = Math.floor(tY / this.tile_size);
  var t_y_down = Math.ceil(tY / this.tile_size);
  var y_near1 = Math.round((this.player.loc.y - offset) / this.tile_size);
  var y_near2 = Math.round((this.player.loc.y + offset) / this.tile_size);

  var t_x_left = Math.floor(tX / this.tile_size);
  var t_x_right = Math.ceil(tX / this.tile_size);
  var x_near1 = Math.round((this.player.loc.x - offset) / this.tile_size);
  var x_near2 = Math.round((this.player.loc.x + offset) / this.tile_size);

  var top1 = this.get_tile(x_near1, t_y_up);
  var top2 = this.get_tile(x_near2, t_y_up);
  var bottom1 = this.get_tile(x_near1, t_y_down);
  var bottom2 = this.get_tile(x_near2, t_y_down);
  var left1 = this.get_tile(t_x_left, y_near1);
  var left2 = this.get_tile(t_x_left, y_near2);
  var right1 = this.get_tile(t_x_right, y_near1);
  var right2 = this.get_tile(t_x_right, y_near2);


  if (tile.jump && this.jump_switch > 15) {

    this.player.can_jump = true;

    this.jump_switch = 0;

  } else this.jump_switch++;

  this.player.vel.x = Math.min(Math.max(this.player.vel.x, -this.current_map.vel_limit.x), this.current_map.vel_limit.x);
  this.player.vel.y = Math.min(Math.max(this.player.vel.y, -this.current_map.vel_limit.y), this.current_map.vel_limit.y);

  this.player.loc.x += this.player.vel.x;
  this.player.loc.y += this.player.vel.y;

  this.player.vel.x *= .9;

  if (left1.solid || left2.solid || right1.solid || right2.solid) {

    /* fix overlap */

    while (this.get_tile(Math.floor(this.player.loc.x / this.tile_size), y_near1).solid
      || this.get_tile(Math.floor(this.player.loc.x / this.tile_size), y_near2).solid)
      this.player.loc.x += 0.1;

    while (this.get_tile(Math.ceil(this.player.loc.x / this.tile_size), y_near1).solid
      || this.get_tile(Math.ceil(this.player.loc.x / this.tile_size), y_near2).solid)
      this.player.loc.x -= 0.1;

    /* tile bounce */

    var bounce = 0;

    if (left1.solid && left1.bounce > bounce) bounce = left1.bounce;
    if (left2.solid && left2.bounce > bounce) bounce = left2.bounce;
    if (right1.solid && right1.bounce > bounce) bounce = right1.bounce;
    if (right2.solid && right2.bounce > bounce) bounce = right2.bounce;

    this.player.vel.x *= -bounce || 0;

  }

  if (top1.solid || top2.solid || bottom1.solid || bottom2.solid) {

    /* fix overlap */

    while (this.get_tile(x_near1, Math.floor(this.player.loc.y / this.tile_size)).solid
      || this.get_tile(x_near2, Math.floor(this.player.loc.y / this.tile_size)).solid)
      this.player.loc.y += 0.1;

    while (this.get_tile(x_near1, Math.ceil(this.player.loc.y / this.tile_size)).solid
      || this.get_tile(x_near2, Math.ceil(this.player.loc.y / this.tile_size)).solid)
      this.player.loc.y -= 0.1;

    /* tile bounce */

    var bounce = 0;

    if (top1.solid && top1.bounce > bounce) bounce = top1.bounce;
    if (top2.solid && top2.bounce > bounce) bounce = top2.bounce;
    if (bottom1.solid && bottom1.bounce > bounce) bounce = bottom1.bounce;
    if (bottom2.solid && bottom2.bounce > bounce) bounce = bottom2.bounce;

    this.player.vel.y *= -bounce || 0;

    if ((bottom1.solid || bottom2.solid) && !tile.jump) {

      this.player.on_floor = true;
      this.player.can_jump = true;
    }

  }

  // adjust camera

  var c_x = Math.round(this.player.loc.x - this.viewport.x / 2);
  var c_y = Math.round(this.player.loc.y - this.viewport.y / 2);
  var x_dif = Math.abs(c_x - this.camera.x);
  var y_dif = Math.abs(c_y - this.camera.y);

  if (x_dif > 5) {

    var mag = Math.round(Math.max(1, x_dif * 0.1));

    if (c_x != this.camera.x) {

      this.camera.x += c_x > this.camera.x ? mag : -mag;

      if (this.limit_viewport) {

        this.camera.x =
          Math.min(
            this.current_map.width_p - this.viewport.x + this.tile_size,
            this.camera.x
          );

        this.camera.x =
          Math.max(
            0,
            this.camera.x
          );
      }
    }
  }

  if (y_dif > 5) {

    var mag = Math.round(Math.max(1, y_dif * 0.1));

    if (c_y != this.camera.y) {

      this.camera.y += c_y > this.camera.y ? mag : -mag;

      if (this.limit_viewport) {

        this.camera.y =
          Math.min(
            this.current_map.height_p - this.viewport.y + this.tile_size,
            this.camera.y
          );

        this.camera.y =
          Math.max(
            0,
            this.camera.y
          );
      }
    }
  }

  if (this.last_tile != tile.id && tile.script) {

    eval(this.current_map.scripts[tile.script]);
  }

  this.last_tile = tile.id;
};

Clarity.prototype.update_player = function () {

  if (this.key.left) {

    if (this.player.vel.x > -this.current_map.vel_limit.x)
      this.player.vel.x -= this.current_map.movement_speed.left;
  }

  if (this.key.up) {

    if (this.player.can_jump && this.player.vel.y > -this.current_map.vel_limit.y) {

      this.player.vel.y -= this.current_map.movement_speed.jump;
      this.player.can_jump = false;
    }
  }

  if (this.key.right) {

    if (this.player.vel.x < this.current_map.vel_limit.x)
      this.player.vel.x += this.current_map.movement_speed.left;
  }

  this.move_player();
};

Clarity.prototype.draw_player = function (context) {

  context.fillStyle = this.player.colour;

  context.beginPath();

  context.arc(
    this.player.loc.x + this.tile_size / 2 - this.camera.x,
    this.player.loc.y + this.tile_size / 2 - this.camera.y,
    this.tile_size / 2 - 1,
    0,
    Math.PI * 2
  );

  context.fill();
};

Clarity.prototype.update = function () {

  this.update_player();
};

Clarity.prototype.draw = function (context) {

  this.draw_map(context, false);
  this.draw_player(context);
};

/* Setup of the engine */

window.requestAnimFrame =
  window.requestAnimationFrame ||
  window.webkitRequestAnimationFrame ||
  window.mozRequestAnimationFrame ||
  window.oRequestAnimationFrame ||
  window.msRequestAnimationFrame ||
  function (callback) {
    return window.setTimeout(callback, 1000 / 60);
  };

var canvas = document.getElementById('canvas'),
  ctx = canvas.getContext('2d');

canvas.width = 400;
canvas.height = 400;

var game = new Clarity();
game.set_viewport(canvas.width, canvas.height);
game.load_map(map);

/* Limit the viewport to the confines of the map */
game.limit_viewport = true;

var Loop = function () {

  ctx.fillStyle = '#333';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  game.update();
  game.draw(ctx);

  window.requestAnimFrame(Loop);
};

Loop();

```

`main.css`

```js

* {
  margin: 0;
  padding: 0;
}

body {
  background: #f2f2f2;
}

canvas {
  display:block;
  margin: 40px auto 20px;
  border: 1px solid #333;
  box-shadow: 0 0 16px 2px rgba(0,0,0,0.8);
}

p, a {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 19px;
  color: #777;
  display: block;
  width: 400px;
  margin: 0 auto;
  text-align: center;
  text-decoration:none;
}

#playername {
  font-size: 24px; /* Adjust the font size as needed */
  font-weight: bold; /* You can make the text bold if you like */
  text-align: center; /* Center the text within the element */
  color: #3498db; /* Change the color to your preference */
  background-color: #f2f2f2; /* Add a background color if desired */
  padding: 10px; /* Add padding to create some spacing around the text */
  border-radius: 5px; /* Add rounded corners to the element */
}

.info {
  margin:14px auto;
  text-align: justify;
  font-size: 18px;
  color: #999;
}

a {
  color:#3377ee;
}
```

`main.mo`

```js
actor {
  public func enterName(name : Text) : async Text {
    let namee : Text = name;
    return "Hello, " # namee # "! You r a good player....";
  };

};

```

# Certification system in REACT

`main.mo`

```js

import HashMap "mo:base/HashMap";
import Principal "mo:base/Principal";
import List "mo:base/List";
import Iter "mo:base/Iter";
import Time "mo:base/Time";
import Debug "mo:base/Debug";

actor certification {

  private stable var registeredEntries : [(Principal, User)] = [];

  // private stable var savedList : [Principal] = [];

  type HashMap<K, V> = HashMap.HashMap<K, V>;

  stable var registedOnesList : List.List<Principal> = List.nil<Principal>();

  var registedUsersHashmap : HashMap<Principal, User> = HashMap.HashMap<Principal, User>(5, Principal.equal, Principal.hash);

  let certificateHolders : HashMap<Principal, Certification> = HashMap.HashMap<Principal, Certification>(5, Principal.equal, Principal.hash);

  // Registeration
  type User = {
    name : Text;
    isRegistered : Bool;
  };

  type Certification = {
    name : Text;
    issueTimestamp : Int;
    isRevoked : Bool;
  };

  public shared (msg) func registerUser(namee : Text) : async Text {

    let obj : User = { name = namee; isRegistered = true };

    let userId : Principal = msg.caller;

    var iter = Iter.fromList(registedOnesList);

    for (user in iter) {
      if (user == userId) {
        return "you are already registered buddy!";
      };
    };

    registedOnesList := Iter.toList(iter);

    registedUsersHashmap.put(userId, obj);
    registedOnesList := List.push(userId, registedOnesList);

    return "success registering!!";

  };

  public func getCertificate(address : Principal) : async Text {

    var iter = Iter.fromList(registedOnesList);
    var userFound : Bool = false;

    let certiName : Text = "Blockchain Panjab Certificate";

    for (user in iter) {
      if (user == address) {

        let obj : Certification = {
          name = certiName;
          issueTimestamp = Time.now();
          isRevoked = false;
        };

        certificateHolders.put(address, obj);

        Debug.print(certiName # "Certificate has been issued!");
        userFound := true;
        // return "you are already registered buddy!";
      } 
    };

    if(userFound == false){
      return "user has not registered himself";
    };

    registedOnesList := Iter.toList(iter);

    return "Certificate has been issued!";

  };

  public shared(msg) func getId(): async Principal{
    return msg.caller;
  };

  public query func getUsers() : async List.List<Principal> {
    return registedOnesList;
  };

  system func preupgrade() {
    registeredEntries := Iter.toArray(registedUsersHashmap.entries());
  };

  system func postupgrade() {

    registedUsersHashmap := HashMap.fromIter<Principal, User>(registeredEntries.vals(), 1, Principal.equal, Principal.hash);


      
    // if (balances.size() < 1) {
    //   balances.put(owner, totalSupply);
    // };
  };

};
```

`index.jsx`

```js

import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(<App />, document.getElementById("root"));
```

`App.jsx`
`
```js

import React from 'react';
import CertificateApp from './CertificateApp';


function App() {
  return (
    <div className="App">
      <CertificateApp/>
    </div>
  );
}

export default App;
```

`CertificateApp.jsx`
`
```js 

import React, { useState } from 'react';
import './CertificateApp.css'; // Import your CSS file
import TextDisplay from './TextDisplay'; // Make sure to provide the correct path
import { certiSys_backend } from '../../../declarations/certiSys_backend';

function CertificateApp() {
  const [name, setName] = useState('');
  const [inputData, setInputData] = useState('');
  const [registeredMsg, setRegisteredMsg] = useState('');
  const [id, setId] = useState('');

  const handleNameChange = async (e) => {
    setName(e.target.value);
  };

  const handleInputDataChange = (e) => {
    setInputData(e.target.value);
  };

  const handleRegister = async () => {
    const regMsg = await certiSys_backend.registerUser(name);
    setRegisteredMsg(regMsg);
  };

  const getIdd = async () => {
    const id = await certiSys_backend.getId();
    setId(id);
  }

  return (
    <div className="container">
      <h1>Register Yourself for a Certification</h1>
      <div>
        <label>
          Enter Your Name:
          <input type="text" value={name} onChange={handleNameChange} />
        </label>
        <button onClick={handleRegister}>Register</button>
      </div>
      {registeredMsg && <div className="result">{registeredMsg}</div>}
      <div className="id-section">
        <h1>Get Your ID</h1>
        {id ? "your id is "+ id : (
          <button className="id-button" onClick={getIdd}>Get ID</button>
        )}
      </div>
      <TextDisplay className="text-display" />
    </div>
  );
}

export default CertificateApp;
```

`TextDisplay.jsx`

```js

import React, { useState } from 'react';
import { certiSys_backend } from '../../../declarations/certiSys_backend';
import {Principal} from '@dfinity/principal'

function TextDisplay() {
  const [inputId, setInputId] = useState('');
  const [displayText, setDisplayText] = useState('');

  const handleIdChange = async (e) => {
    setInputId(e.target.value);
   
  };

  const handleDisplayText = async () => {
    console.log(inputId)

    const message = await certiSys_backend.getCertificate(Principal.fromText(inputId));
    console.log(message);
    setDisplayText(message);
    console.log(displayText);
   
  };

  return (
    <div>
      <h1>Get your certificate !! </h1>
      <div>
        <label>
          Enter your ID :
          <input type="text" value={inputId} onChange={handleIdChange} />
        </label>
        <button onClick={handleDisplayText}>Get Certificate!</button>
      </div>
      {displayText && (
        <div>
          <p>Text Displayed: {displayText} </p>
        </div>
      )}
    </div>
  );
}

export default TextDisplay;

```

`package.json`

```js
// inside package.json update depedencies

"dependencies": {
    "@dfinity/agent": "^0.19.3",
    "@dfinity/candid": "^0.19.3",
    "@dfinity/principal": "^0.19.3",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@material-ui/core": "^4.12.4",
    "@material-ui/icons": "^4.11.3",
    "@mui/icons-material": "^5.14.14",
    "@mui/material": "^5.14.14",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "ts-loader": "^9.5.0"
  }
```
`index.html`

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Document</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  	<link id="external-css" rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Montserrat&amp;display=swap" media="all">
</head>
  <body>
    <div id="root"></div>
  </body>
</html>

```

## FLAPPY BIRD GAME 

`main.mo`

```js
actor {
  type BoxInputData = {
    box0 : Text;
    box1 : Text;
    box2 : Text;
  };

  public query func changeTurn(turn : Text) : async Text {
    var result : Text = "";
    if (turn == "X") {
      result := "0";
    } else {
      result := "X";
    };
    return result;
  };

  public func checkWin(data : BoxInputData) : async Bool {
    if ((data.box0 == data.box1) and (data.box2 == data.box1) and (data.box0 != "")) {
      return true;
    };
    return false;
  };
};
```

`index.html`

```js
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" type="text/css" href="main.css" />
  </head>
  <body onload="startGame()">
    <h1>Flappy Bird Game</h1>
    <script>
      var myGamePiece;
      var myObstacles = [];
      var myScore;
      function startGame() {
        myGamePiece = new component(30, 30, "red", 10, 120);
        myGamePiece.gravity = 0.05;
        myScore = new component("30px", "Consolas", "black", 280, 40, "text");
        myGameArea.start();
      }

      var myGameArea = {
        canvas: document.createElement("canvas"),
        start: function () {
          this.canvas.width = 480;
          this.canvas.height = 270;
          this.context = this.canvas.getContext("2d");
          document.body.insertBefore(this.canvas, document.body.childNodes[0]);
          this.frameNo = 0;
          this.interval = setInterval(updateGameArea, 20);
        },
        clear: function () {
          this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
        },
      };

      function component(width, height, color, x, y, type) {
        this.type = type;
        this.score = 0;
        this.width = width;
        this.height = height;
        this.speedX = 0;
        this.speedY = 0;
        this.x = x;
        this.y = y;
        this.gravity = 0;
        this.gravitySpeed = 0;
        this.update = function () {
          ctx = myGameArea.context;
          if (this.type == "text") {
            ctx.font = this.width + " " + this.height;
            ctx.fillStyle = color;
            ctx.fillText(this.text, this.x, this.y);
          } else {
            ctx.fillStyle = color;
            ctx.fillRect(this.x, this.y, this.width, this.height);
          }
        };
        this.newPos = function () {
          this.gravitySpeed += this.gravity;
          this.x += this.speedX;
          this.y += this.speedY + this.gravitySpeed;
          this.hitBottom();
        };
        this.hitBottom = function () {
          var rockbottom = myGameArea.canvas.height - this.height;
          if (this.y > rockbottom) {
            this.y = rockbottom;
            this.gravitySpeed = 0;
          }
        };
        this.crashWith = function (otherobj) {
          var myleft = this.x;
          var myright = this.x + this.width;
          var mytop = this.y;
          var mybottom = this.y + this.height;
          var otherleft = otherobj.x;
          var otherright = otherobj.x + otherobj.width;
          var othertop = otherobj.y;
          var otherbottom = otherobj.y + otherobj.height;
          var crash = true;
          if (
            mybottom < othertop ||
            mytop > otherbottom ||
            myright < otherleft ||
            myleft > otherright
          ) {
            crash = false;
          }
          return crash;
        };
      }

      function updateGameArea() {
        var x, height, gap, minHeight, maxHeight, minGap, maxGap;
        for (i = 0; i < myObstacles.length; i += 1) {
          if (myGamePiece.crashWith(myObstacles[i])) {
            return;
          }
        }
        myGameArea.clear();
        myGameArea.frameNo += 1;
        if (myGameArea.frameNo == 1 || everyinterval(150)) {
          x = myGameArea.canvas.width;
          minHeight = 20;
          maxHeight = 200;
          height = Math.floor(
            Math.random() * (maxHeight - minHeight + 1) + minHeight
          );
          minGap = 50;
          maxGap = 200;
          gap = Math.floor(Math.random() * (maxGap - minGap + 1) + minGap);
          myObstacles.push(new component(10, height, "green", x, 0));
          myObstacles.push(
            new component(10, x - height - gap, "green", x, height + gap)
          );
        }
        for (i = 0; i < myObstacles.length; i += 1) {
          myObstacles[i].x += -1;
          myObstacles[i].update();
        }
        myScore.text = "SCORE: " + myGameArea.frameNo;
        myScore.update();
        myGamePiece.newPos();
        myGamePiece.update();
      }

      function everyinterval(n) {
        if ((myGameArea.frameNo / n) % 1 == 0) {
          return true;
        }
        return false;
      }

      function accelerate(n) {
        myGamePiece.gravity = n;
      }
    </script>
    <br />
    <button onmousedown="accelerate(-0.2)" onmouseup="accelerate(0.05)">
      ACCELERATE
    </button>
    <p>Use the ACCELERATE button to stay in the air</p>
    <p>How long can you stay alive?</p>
  </body>
</html>

```

`main.css`

```js
canvas {
  border: 1px solid #d3d3d3;
  background-color: #f1f1f1;
}
body {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
}
h1 {
  font-size: 36px;
}
button {
  padding: 10px 20px;
  background-color: #4caf50;
  border: none;
  color: white;
  font-size: 18px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
p {
  margin-top: 10px;
  font-size: 18px;
}
```


## MEMORY CARD GAME

`main.mo`

```js
actor {
  type BoxInputData = {
    box0 : Text;
    box1 : Text;
    box2 : Text;
  };

  public query func changeTurn(turn : Text) : async Text {
    var result : Text = "";
    if (turn == "X") {
      result := "0";
    } else {
      result := "X";
    };
    return result;
  };

  public func checkWin(data : BoxInputData) : async Bool {
    if ((data.box0 == data.box1) and (data.box2 == data.box1) and (data.box0 != "")) {
      return true;
    };
    return false;
  };
};
```
`index.html`

```js
<!DOCTYPE html>
<!-- Coding By CodingNepal - youtube.com/codingnepal -->
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">  
    <title>Memory Card Game in JavaScript | CodingNepal</title>
    <link rel="stylesheet" href="main.css">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <div class="wrapper">
      <h1>Memory Card Game</h1>
      <ul class="cards">
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-1.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-6.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-3.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-2.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-1.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-5.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-2.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-6.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-3.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-5.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
        <li class="card">
          <div class="view front-view">
            <img src="images/que_icon.svg" alt="icon">
          </div>
          <div class="view back-view">
            <img src="images/img-4.png" alt="card-img">
          </div>
        </li>
      </ul>
    </div>

    <script src="index.js"></script>

  </body>
</html>
```
`index.js`

```js
const cards = document.querySelectorAll(".card");

let matched = 0;
let cardOne, cardTwo;
let disableDeck = false;

function flipCard({target: clickedCard}) {
    if(cardOne !== clickedCard && !disableDeck) {
        clickedCard.classList.add("flip");
        if(!cardOne) {
            return cardOne = clickedCard;
        }
        cardTwo = clickedCard;
        disableDeck = true;
        let cardOneImg = cardOne.querySelector(".back-view img").src,
        cardTwoImg = cardTwo.querySelector(".back-view img").src;
        matchCards(cardOneImg, cardTwoImg);
    }
}

function matchCards(img1, img2) {
    if(img1 === img2) {
        matched++;
        if(matched == 8) {
            setTimeout(() => {
                return shuffleCard();
            }, 1000);
        }
        cardOne.removeEventListener("click", flipCard);
        cardTwo.removeEventListener("click", flipCard);
        cardOne = cardTwo = "";
        return disableDeck = false;
    }
    setTimeout(() => {
        cardOne.classList.add("shake");
        cardTwo.classList.add("shake");
    }, 400);

    setTimeout(() => {
        cardOne.classList.remove("shake", "flip");
        cardTwo.classList.remove("shake", "flip");
        cardOne = cardTwo = "";
        disableDeck = false;
    }, 1200);
}

function shuffleCard() {
    matched = 0;
    disableDeck = false;
    cardOne = cardTwo = "";
    let arr = [1, 2, 3, 4, 5, 6, 7, 8, 1, 2, 3, 4, 5, 6, 7, 8];
    arr.sort(() => Math.random() > 0.5 ? 1 : -1);
    cards.forEach((card, i) => {
        card.classList.remove("flip");
        let imgTag = card.querySelector(".back-view img");
        imgTag.src = `images/img-${arr[i]}.png`;
        card.addEventListener("click", flipCard);
    });
}

shuffleCard();
    
cards.forEach(card => {
    card.addEventListener("click", flipCard);
});
```

`main.css`

```js
/* Import Google Font - Poppins */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}
body{
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #6563FF;
}
.wrapper{
  padding: 25px;
  border-radius: 10px;
  background: #F8F8F8;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}
.cards, .card, .view{
  display: flex;
  align-items: center;
  justify-content: center;
}
.cards{
  height: 400px;
  width: 400px;
  flex-wrap: wrap;
  justify-content: space-between;
}
.cards .card{
  cursor: pointer;
  list-style: none;
  user-select: none;
  position: relative;
  perspective: 1000px;
  transform-style: preserve-3d;
  height: calc(100% / 4 - 10px);
  width: calc(100% / 4 - 10px);
}
.card.shake{
  animation: shake 0.35s ease-in-out;
}
@keyframes shake {
  0%, 100%{
    transform: translateX(0);
  }
  20%{
    transform: translateX(-13px);
  }
  40%{
    transform: translateX(13px);
  }
  60%{
    transform: translateX(-8px);
  }
  80%{
    transform: translateX(8px);
  }
}
.card .view{
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 7px;
  background: #fff;
  pointer-events: none;
  backface-visibility: hidden;
  box-shadow: 0 3px 10px rgba(0,0,0,0.1);
  transition: transform 0.25s linear;
}
.card .front-view img{
  width: 19px;
}
.card .back-view img{
  max-width: 45px;
}
.card .back-view{
  transform: rotateY(-180deg);
}
.card.flip .back-view{
  transform: rotateY(0);
}
.card.flip .front-view{
  transform: rotateY(180deg);
}

@media screen and (max-width: 700px) {
  .cards{
    height: 350px;
    width: 350px;
  }
  .card .front-view img{
    width: 17px;
  }
  .card .back-view img{
    max-width: 40px;
  }
}

@media screen and (max-width: 530px) {
  .cards{
    height: 300px;
    width: 300px;
  }
  .card .front-view img{
    width: 15px;
  }
  .card .back-view img{
    max-width: 35px;
  }
}
```
`image folder`

```js
// in this game we have one folder of images which you have to add in assets , 

// just download image folder from assets and place it inside your project assets

```

## Mobile-Calcy in REACT

`main.mo`

```js
import Float "mo:base/Float";
actor Calculator {

  var num : Float = 0;

  public func add(val1 : Float, val2 : Float) : async Float {
    num := val1 +val2;
    return num;
  };

  public func subtract(val1 : Float, val2 : Float) : async Float {
    num := val1 -val2;
    return num;
  };

  public func multiply(val1 : Float, val2 : Float) : async Float {
    num := val1 * val2;
    return num;
  };

  public func divison(val1 : Float, val2 : Float) : async ?Float {
    if (val2 == 0) {
      return null;
    } else {
      num := val1 / val2;
      return ?num;
    };
  };

  public func clearAll() : async Float {
    num := 0;
    return num;
  };

};
```

`index.html`

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>calcy</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  	<link id="external-css" rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Montserrat&amp;display=swap" media="all">
</head>
  <body>
    <div id="root"></div>
  </body>
</html>

```

`index.jsx`

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./Components/App";

ReactDOM.render(<App />, document.getElementById("root"))

```

`App.jsx`

```js
import React, { useState } from "react";
import { calcy_backend } from "../../../declarations/calcy_backend";

function App() {
  const [curr, setCurr] = useState("");
  const [prev, setPrev] = useState("");
  const [operand, setOperand] = useState("");

  const numHandler = (e) => {
    if (curr.includes(".") && e.target.value === ".") return;
    curr ? setCurr((prev) => prev + e.target.value) : setCurr(e.target.value);
  };

  const clearHandler = async () => {
    await calcy_backend.clearAll();
    setCurr("");
    setPrev("");
    setOperand("");
  };
  const clearLastHandler = () => {
    if (operand) return;
    let res = curr.slice(0, -1);
    setCurr(res);
  };

  const operandHandler = (e) => {
    setOperand(e.target.value);
    if (curr === "") return;
    if (prev !== "") calculate();
    else {
      setPrev(curr);
      setCurr("");
    }
  };
  const calculate = async () => {
    let cal, val1, val2;
    switch (operand) {
      case "+":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.add(val1, val2));
        break;

      case "-":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.subtract(val1, val2));
        break;

      case "*":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.multiply(val1, val2));
        break;

      case "/":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.divison(val1, val2));
        break;
      default:
        return;
    }
    console.log("cal", cal);

    setPrev(cal);
    setCurr("");
  };

  let numArray = [];
  for (let i = 0; i <= 9; i++) {
    numArray.push(i);
  }
  numArray.push(".");

  const operandsss = ["+", "-", "*", "/"];

  return (
    <div className="App">
      <h1>calculator</h1>
      <div className="screen">
        <span>{prev}</span>
        <span>{operand}</span>
        <span>{curr}</span>
      </div>
      {operandsss.map((opr, index) => (
        <button
          key={index}
          value={opr}
          className="button opr"
          onClick={operandHandler}
        >
          {opr}
        </button>
      ))}
      {numArray.map((num, index) => (
        <button key={index} value={num} className="button" onClick={numHandler}>
          {num}
        </button>
      ))}
      <input
        type="button"
        value="X"
        className="button btn2"
        onClick={clearLastHandler}
      />
      <input
        type="button"
        value="AC"
        className="button btn"
        onClick={clearHandler}
      />
      <input
        type="button"
        value="="
        className="button button1"
        onClick={calculate}
      />
    </div>
  );
}

export default App;
```

`main.css`

```js
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}

.App {
  width: 390px;
  margin: 100px auto;
  align-items: center;
}

.screen {
  width: 300px;
  height: 80px;
  text-align: right;
  border: 3px solid rgb(88, 87, 87);
  font-size: 30px;
  padding: 10px;
  border-radius: 10px;
  background-color: white;
}

.button {
  width: 80px;
  height: 80px;
  background-color: black;
  color: white;
  border-radius: 10px;
  font-size: 25px;
}

body {
  background-color: lightgray;
}

.btn {
  width: 160px;
  background-color: white;
  color: black;
}

.opr {
  background-color: rgb(158, 55, 7);
}

.btn2 {
  background-color: rgb(65, 63, 63);
}

.button1 {
  width: 160px;
  background-color: yellowgreen;
}
```

`package.json`

```js
// change your depedencies with this in package.json

"dependencies": {
    "@dfinity/agent": "^0.19.3",
    "@dfinity/candid": "^0.19.3",
    "@dfinity/principal": "^0.19.3",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@material-ui/core": "^4.12.4",
    "@material-ui/icons": "^4.11.3",
    "@mui/icons-material": "^5.14.14",
    "@mui/material": "^5.14.14",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "ts-loader": "^9.5.0"
  },
```
## todo list without login 

`main.mo`

```js
import Text "mo:base/Text";
import List "mo:base/List";
import Debug "mo:base/Debug";
import Nat "mo:base/Nat";

actor {

  public type Note = {
    textarea : Text;
    id : Nat;
  };

  var notes : List.List<Note> = List.nil<Note>();

  public func addTodo(title : Text) : async Nat {
    let todoId = List.size(notes) +1;

    let newNote : Note = {
      id = todoId;
      textarea = title;
    };

    notes := List.push(newNote, notes);
    Debug.print(debug_show (notes));

    return newNote.id;
  };

  public query func getAllTodo() : async [Note] {
    return List.toArray(notes);

  };

  public func deleteTodo(id : Nat) {
    Debug.print(debug_show (id));

    notes := List.filter(
      notes,
      func(note : Note) : Bool {
        return note.id != id;
      },
    );

    Debug.print("id is deleted");
  };

};

```

`index.html`

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="main.css" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css"
    />
    <title>ToDo List</title>
  </head>
  <body>
    <div class="container-title">
      <h1>TO DO LIST</h1>
    </div>
    <div class="container">
      <form class="inputcol">
        <textarea
          class="textarea"
          type="text"
          name="textarea"
          placeholder="Add todo"
          required
        ></textarea>
        <button class="buttoninput" type="submit">
          <i class="fa-solid fa-check"></i>
        </button>
      </form>
    </div>
    <ul id="todolist"></ul>
    <script src="index.js"></script>
  </body>
</html>

```

`index.js`

```js
import { todo_backend } from "../../declarations/todo_backend";

const textarea = document.querySelector(".textarea");
const appointForm = document.querySelector(".inputcol");

appointForm.addEventListener("submit", async (e) => {
  e.preventDefault();
  const obj = {
    textarea: textarea.value,
  };

  try {
    const getId = await todo_backend.addTodo(obj.textarea);
    showUserOnScreen({ id: getId, textarea: obj.textarea });
    textarea.value = ""; 
  } catch (error) {
    console.error("Error adding todo:", error);
  }
});

window.addEventListener("DOMContentLoaded", async () => {
  try {
    const getAll = await todo_backend.getAllTodo();
    getAll.forEach((todo) => showUserOnScreen(todo));
  } catch (error) {
    console.error("Error fetching todo list:", error);
  }
});

function showUserOnScreen(user) {
  const parentNode = document.getElementById("todolist");

  const newListItem = document.createElement("li");
  newListItem.className = "item";
  newListItem.id = Number(user.id);
  newListItem.textContent = user.textarea;

  const deleteButton = document.createElement("button");
  deleteButton.className = "trash-button";
  deleteButton.innerHTML = '<i class="fa-solid fa-trash"></i>';
  deleteButton.onclick = () => deleteTodo(user.id);

  newListItem.appendChild(deleteButton);
  parentNode.appendChild(newListItem);
}

async function deleteTodo(userId) {
  try {
    console.log("userId in del =>", userId);
    await todo_backend.deleteTodo(userId);
    removeUserFromScreen(userId);
  } catch (error) {
    console.error("Error deleting todo:", error);
  }
}

function removeUserFromScreen(userId) {
  const childNodeToBeDeleted = document.getElementById(userId);
  if (childNodeToBeDeleted) {
    childNodeToBeDeleted.parentNode.removeChild(childNodeToBeDeleted);
  }
}

```
`main.css`

```js
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Times New Roman", Times, serif;
}

body {
  background-image: linear-gradient(to right, #605e7e, #6b73c1, #37b3cc);
  margin: 50px 2%;
}

.container-title {
  font-size: 25px;
  text-align: center;
  margin-bottom: 30px;
  color: white;
  text-shadow: 3px 1px black;
}

.inputcol {
  display: grid;
  column-gap: 5px;
  grid-template-columns: 60% 10%;
  justify-content: center;
  margin-top: 10px;
  margin-bottom: 20px;
}

.textarea {
  min-height: 50px;
  max-width: 100%;
  border-radius: 10px;
  border-color: #333;
  font-size: 20px;
  padding: 10px;
  overflow: auto;
  overflow-x: hidden;
}

.textarea::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: #333;
}

.textarea::-webkit-scrollbar {
  width: 10px;
  cursor: pointer;
}

.textarea::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #8f8acc;
}

.textarea:focus {
  outline: none;
}

.buttoninput {
  border-radius: 10px;
  border-color: #333;
  background-color: white;
  font-size: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.buttoninput:hover {
  background-color: #8f8acc;
  color: white;
}

#todolist .item {
  background-color: #fff;
  border-radius: 8px;
  margin-bottom: 10px;
  padding: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  word-wrap: break-word;
  word-break: break-all;
  font-size: 20px;
}

.trash-button {
  background-color: transparent;
  color: black;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.trash-button:hover {
  background-color: #6b73c1;
  color: white;
  transform: scale(1.05);
}

.trash-button:active {
  transform: scale(0.95);
}

.fa-check,
.fa-trash {
  pointer-events: none;
}

```