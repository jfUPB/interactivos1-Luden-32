## server.js
```js
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);
const path = require('path');

app.use(express.static('views'));

app.get('/page1', (req, res) => {
    res.sendFile(path.join(__dirname, 'views/page1.html'));
});

app.get('/page2', (req, res) => {
    res.sendFile(path.join(__dirname, 'views/page2.html'));
});

let scores = { player1: 0, player2: 0 };

io.on('connection', (socket) => {
    console.log('A user connected:', socket.id);

    socket.on('keypress', (player) => {
        scores[player]++;
        io.emit('scoreUpdate', scores);

        if (scores[player] >= 20) {
            io.emit('winner', player);
            scores = { player1: 0, player2: 0 };
        }
    });
});

http.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

## views/page1.js
```js
// Conexión al servidor
const socket = io();

// Cada vez que el usuario presiona la barra espaciadora...
document.addEventListener('keydown', (e) => {
  if (e.code === 'Space') {
    socket.emit('keypress', 'player1');
  }
});

socket.on('scoreUpdate', (scores) => {
  document.getElementById('scores').innerText =
    `P1: ${scores.player1} | P2: ${scores.player2}`;
});

socket.on('winner', (player) => {
  document.getElementById('winner').innerText =
    `El ganador es ${player}`;
});
```
views/page2.js
```js
// Conexión al servidor
const socket = io();

// Escuchamos la barra espaciadora para el jugador 2
document.addEventListener('keydown', (e) => {
  if (e.code === 'Space') {
    socket.emit('keypress', 'player2');
  }
});

socket.on('scoreUpdate', (scores) => {
  document.getElementById('scores').innerText =
    `P1: ${scores.player1} | P2: ${scores.player2}`;
});

socket.on('winner', (player) => {
  document.getElementById('winner').innerText =
    `El ganador es ${player}`;
});
```
views/page1.html
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>ClickWar – Player 1</title>
  <style>
    body { font-family: sans-serif; text-align: center; margin-top: 50px; }
    #instructions { margin-bottom: 20px; font-size: 1.2em; }
  </style>
</head>
<body>
  <h1>Player 1</h1>
  <div id="instructions">Presiona la barra espaciadora para sumar puntos</div>
  <h2 id="scores">Esperando datos</h2>
  <h2 id="winner">...</h2>

  <script src="/socket.io/socket.io.js"></script>
  <script src="page1.js"></script>
</body>
</html>
```
## views/page2.html  
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>ClickWar – Player 2</title>
  <style>
    body { font-family: sans-serif; text-align: center; margin-top: 50px; }
    #instructions { margin-bottom: 20px; font-size: 1.2em; }
  </style>
</head>
<body>
  <h1>Player 2</h1>
  <div id="instructions">Presiona la barra espaciadora para sumar puntos</div>
  <h2 id="scores">Esperando datos</h2>
  <h2 id="winner">...</h2>

  <script src="/socket.io/socket.io.js"></script>
  <script src="page2.js"></script>
</body>
</html>
```
