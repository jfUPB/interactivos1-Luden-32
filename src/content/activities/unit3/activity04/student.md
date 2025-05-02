```js
let serial;
let btnConnect, btnA, btnB, btnShake, btnTouch;

function setup() {
  createCanvas(400, 200);
  
    serial = createSerial();
  
  // Botón de conexión serial
  btnConnect = createButton('Conectar Serial');
  btnConnect.position(20, 20);
  btnConnect.mousePressed(toggleSerial);
  
  // Botones de control
  btnA = createButton('A (+ tiempo)');
  btnA.position(20, 60);
  btnA.mousePressed(() => sendCommand('A'));
  
  btnB = createButton('B (- tiempo)');
  btnB.position(120, 60);
  btnB.mousePressed(() => sendCommand('B'));
  
  btnShake = createButton('SHAKE (Armar)');
  btnShake.position(20, 100);
  btnShake.mousePressed(() => sendCommand('S'));
  
  btnTouch = createButton('TOUCH (Reiniciar)');
  btnTouch.position(180, 100);
  btnTouch.mousePressed(() => sendCommand('T'));

  // Opcional: recibir datos del micro:bit
  serial.on('data', () => {
    let data = serial.read();
    console.log("Recibido:", data);
  });
}

function draw() {
  background(220);
  // Mostrar estado de conexión
  fill(0);
  text("Estado: " + (serial.isOpen() ? "Conectado" : "Desconectado"), 20, 160);
}

function toggleSerial() {
  if (!serial.isOpen()) {
    serial.requestPort();
  } else {
    serial.close();
  }
}

function sendCommand(cmd) {
  if (serial.isOpen()) {
    serial.write(cmd + '\n');  // Enviar comando con salto de línea
    console.log("Enviado:", cmd);
  }
}
```
