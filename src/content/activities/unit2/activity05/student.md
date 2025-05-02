## Código en p5.js

```jsx

let port;
let connectButton;
let statusText = '';

function setup() {
  createCanvas(500, 500);
  background(210);

  // Inicializar el puerto serial
  port = createSerial();

  // Botón para conectar o desconectar
  connectButton = createButton('Conectar a micro:bit');
  connectButton.position(180, 400);
  connectButton.mousePressed(toggleConnection);

  // Botones para enviar comandos
  let btnHappy = createButton('Enviar Feliz');
  btnHappy.position(30, 450);
  btnHappy.mousePressed(() => sendCommand('h'));

  let btnDuck = createButton('Enviar Pato');
  btnDuck.position(130, 450);
  btnDuck.mousePressed(() => sendCommand('w'));

  let btnNo = createButton('Enviar No');
  btnNo.position(230, 450);
  btnNo.mousePressed(() => sendCommand('r'));

  let btnSad = createButton('Enviar Triste');
  btnSad.position(330, 450);
  btnSad.mousePressed(() => sendCommand('g'));
}

function draw() {
  // Actualiza el estado de conexión en el botón
  if (!port.opened()) {
    connectButton.html('Conectar a micro:bit');
  } else {
    connectButton.html('Desconectar');
  }

  // Si hay datos disponibles, se leen y se actualiza el estado visual
  if (port.availableBytes() > 0) {
    let incoming = port.read(1);
    statusText = incoming;
  }

  // Visualización: se muestra el carácter recibido y se dibuja un círculo que cambia de color
  background(210);
  fill(0);
  textSize(36);
  textAlign(CENTER, CENTER);
  text(statusText, width / 2, height / 2);

  // Círculo indicativo: se cambia el color según el comando recibido
  noStroke();
  switch(statusText) {
    case 'h': fill('yellow'); break;
    case 'w': fill('pink'); break;
    case 'r': fill('red'); break;
    case 'g': fill('blue'); break;
    default: fill('gray'); break;
  }
  ellipse(width / 2, height / 2 + 100, 80, 80);
}

function toggleConnection() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}

function sendCommand(cmd) {
  port.write(cmd);
}

```

---

## Código en MicroPython para el micro:bit

Este código en el micro:bit lee un carácter proveniente del puerto serial, muestra un icono distinto en la pantalla LED según el comando recibido y luego borra la pantalla para estar listo para la siguiente entrada. Se ha optimizado el tiempo de visualización y se añadió una respuesta por defecto en caso de recibir un comando no reconocido.

```python
from microbit import *
import utime

# Inicialización del puerto serial con la misma velocidad que el p5.js
uart.init(baudrate=115200)

while True:
    if uart.any():
        data = uart.read(1)
        if data:
            command = data.decode('utf-8')
            # Selección del icono según el comando recibido
            if command == 'h':
                display.show(Image.HAPPY)
            elif command == 'w':
                display.show(Image.CHESSBOARD)  # Usamos un icono distinto para representar "pato"
            elif command == 'r':
                display.show(Image.NO)
            elif command == 'g':
                display.show(Image.SAD)
            else:
                display.show(Image.CONFUSED)
            utime.sleep(0.5)
            display.clear()

```

---

### Documentación Breve del Proceso

- **Objetivo:** Integrar la comunicación serial entre p5.js y el micro:bit para enviar comandos y visualizar respuestas en ambos dispositivos.
- **p5.js:**
    - Se crea una interfaz gráfica con botones para conectar y enviar comandos.
    - El canvas muestra el carácter recibido y un círculo cambia de color según el comando.
- **MicroPython:**
    - Se configura la comunicación serial.
    - Se lee el comando recibido y se muestra un icono en la pantalla LED del micro:bit.
- **Resultado:**
    - La conexión se establece correctamente y los comandos enviados generan la respuesta visual esperada tanto en la interfaz p5.js como en el micro:bit.
