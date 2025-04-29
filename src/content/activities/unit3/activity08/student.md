## p5.js
```js
let currentState;
let timeLeft;
let deactStep;
let lastUpdate;
let explosionSound;
let armedStartTime;

// Estados
const STATE_CONFIG = 0;
const STATE_ARMED = 1;
const STATE_COUNTDOWN = 2;
const STATE_EXPLODED = 3;

function preload() {
  explosionSound = loadSound('https://assets.mixkit.co/active_storage/sfx/2743/2743-preview.mp3');
}

function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);
  resetBomb();
}

function resetBomb() {
  currentState = STATE_CONFIG;
  timeLeft = 20;
  deactStep = 0;
  lastUpdate = millis();
  armedStartTime = 0;
}

function draw() {
  background(220);
  
  // Máquina de estados
  switch(currentState) {
    case STATE_CONFIG:
      drawConfig();
      break;
    case STATE_ARMED:
      drawArmed();
      break;
    case STATE_COUNTDOWN:
      drawCountdown();
      break;
    case STATE_EXPLODED:
      drawExploded();
      break;
  }
}

function drawConfig() {
  fill(0);
  text("CONFIGURACIÓN", width/2, 50);
  text("Tiempo: " + timeLeft, width/2, height/2);
  text("A: +1s  B: -1s  S: Armar", width/2, height - 50);
}

function drawArmed() {
  fill(0, 255, 0);
  ellipse(width/2, height/2, 100, 100);
  fill(0);
  text("ARMADA", width/2, height - 50);
  
  // Esperar 1 segundo
  if (millis() - armedStartTime >= 1000) {
    currentState = STATE_COUNTDOWN;
    deactStep = 0;
  }
}

function drawCountdown() {
  // Actualizar tiempo cada segundo
  if (millis() - lastUpdate >= 1000) {
    timeLeft--;
    lastUpdate = millis();
  }
  
  // Dibujar cuenta regresiva
  fill(255, 0, 0);
  rect(50, height/2 - 20, 300 * (timeLeft/20), 40);
  fill(0);
  text(timeLeft, width/2, 100);
  
  // Verificar explosión
  if (timeLeft <= 0) {
    currentState = STATE_EXPLODED;
    if (!explosionSound.isPlaying()) {
      explosionSound.play();
    }
  }
  
  // Instrucciones desactivación
  text("Secuencia: A-B-A-S", width/2, height - 80);
  text("Touch (click) para reiniciar", width/2, height - 40);
}

function drawExploded() {
  fill(0);
  textSize(40);
  text("💀 EXPLOSIÓN 💀", width/2, height/2);
  textSize(20);
  text("Haz click para reiniciar", width/2, height - 50);
}

function keyPressed() {
  if (currentState === STATE_CONFIG) {
    if (key === 'A' && timeLeft < 60) {
      timeLeft++;
    } else if (key === 'B' && timeLeft > 10) {
      timeLeft--;
    } else if (key === 'S' || keyCode === 32) { // 32 = espacio (shake)
      currentState = STATE_ARMED;
      armedStartTime = millis();
    }
  }
  
  if (currentState === STATE_COUNTDOWN) {
    handleDeactivationSequence(key.toUpperCase());
  }
}

function handleDeactivationSequence(key) {
  switch(deactStep) {
    case 0:
      if (key === 'A') deactStep = 1;
      else deactStep = 0;
      break;
    case 1:
      if (key === 'B') deactStep = 2;
      else deactStep = 0;
      break;
    case 2:
      if (key === 'A') deactStep = 3;
      else deactStep = 0;
      break;
    case 3:
      if (key === 'S') {
        resetBomb();
        currentState = STATE_CONFIG;
      } else {
        deactStep = 0;
      }
      break;
  }
}

function mousePressed() {
  if (currentState === STATE_EXPLODED || 
     (currentState === STATE_COUNTDOWN && keyIsPressed)) {
    resetBomb();
  }
}
```

## microbit
```py
from microbit import *

while True:
    if button_a.was_pressed():
        uart.write("A\n")
    if button_b.was_pressed():
        uart.write("B\n")
    if accelerometer.was_gesture("shake"):
        uart.write("S\n")
    if pin_logo.is_touched():
        uart.write("T\n")

    display.show(Image.HEART)
    sleep(100)
```
