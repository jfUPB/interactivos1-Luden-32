## p5.js
```js
let serial;    // Objeto para la comunicación serial
let circleX;   // Posición en x del círculo

function setup() {
  createCanvas(400, 400);
  // Inicializamos la posición del círculo en el centro del canvas
  circleX = width / 2;
  
  // Creamos una instancia de p5.WebSerial
  serial = new p5.WebSerial();
  
  // Configuramos el callback para recibir datos del micro:bit
  serial.on("data", gotData);
  
  // Solicitamos apertura del puerto serial.
  // Debido a restricciones de seguridad, el usuario debe interactuar (clic en pantalla) para abrir el puerto.
  serial.open();
}

function draw() {
  background(220);
  
  // Dibujamos el círculo (radio 25)
  fill(0, 0, 255);
  noStroke();
  ellipse(circleX, height / 2, 50, 50);
}

// Esta función se ejecuta cuando se reciben datos del micro:bit.
function gotData() {
  // Leemos una línea completa de datos
  let data = serial.readLine();
  if (data) {
    data = data.trim(); // Limpiamos espacios y saltos de línea
    console.log("Dato recibido:", data);
    
    // Según el comando recibido, movemos el círculo en la dirección correspondiente
    if (data === "left") {
      circleX -= 10; // Mover a la izquierda
    } else if (data === "right") {
      circleX += 10; // Mover a la derecha
    }
    
    // Nos aseguramos de que el círculo se mantenga dentro del canvas
    circleX = constrain(circleX, 25, width - 25);
  }
}
```

## micro:bit
```python
from microbit import *

# Inicializamos la comunicación serial con una tasa de baudios de 115200
uart.init(baudrate=115200)

while True:
    if button_a.was_pressed():
        uart.write("left\n")
        sleep(200)  # Pequeña espera para evitar rebotes
    if button_b.was_pressed():
        uart.write("right\n")
        sleep(200)
```

## Explicación del mapeo de botones al movimiento del círculo
### Micro:bit y envío de comandos:

* Botón A: Cuando se detecta que el botón A ha sido presionado, el micro:bit envía la cadena "left\n" a través del puerto serial.
* Botón B: De manera similar, al presionar el botón B se envía "right\n".
### Recepción y procesamiento en p5.js:

* El programa en p5.js utiliza la librería p5.webserial para abrir y gestionar la comunicación serial.
* La función gotData se activa cada vez que se recibe una línea de datos.
* Se lee el mensaje recibido, se limpia de espacios y se evalúa:
  * Si el mensaje es "left", se resta un valor fijo a la posición en x del círculo, moviéndolo a la izquierda.
  * Si el mensaje es "right", se suma ese mismo valor, desplazándolo a la derecha.
* Se utiliza constrain para asegurarse de que el círculo no salga de los límites del canvas.
