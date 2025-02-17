## p5.js
```js
let serial;         // Variable para manejar la comunicación serial
let squareColor;    // Color actual del cuadrado

function setup() {
  createCanvas(400, 400);
  // Color inicial del cuadrado
  squareColor = color(255, 0, 0); // rojo

  // Crear una instancia de p5.WebSerial
  serial = new p5.WebSerial();

  // Configurar el callback para cuando se reciban datos
  serial.on("data", gotData);

  // Iniciar la conexión serial. Debido a restricciones de seguridad, es probable
  // que necesites una interacción del usuario (como un clic) para que se abra el puerto.
  serial.open();
}

function draw() {
  background(220);
  fill(squareColor);
  rect(100, 100, 200, 200);
}

// Esta función se llama cada vez que se reciben datos del micro:bit.
function gotData() {
  // Lee la línea completa de datos
  let data = serial.readLine();
  if (data) {
    // Eliminar espacios en blanco y saltos de línea
    data = data.trim();
    console.log("Datos recibidos:", data);
    // Si el mensaje es "change", se cambia el color del cuadrado
    if (data === "change") {
      // Cambiar a un color aleatorio
      squareColor = color(random(255), random(255), random(255));
    }
  }
}
```

## micro:bit Python
```python
from microbit import *

# Inicializa el puerto serial con una tasa de baudios de 115200
uart.init(baudrate=115200)

while True:
    if button_a.was_pressed():
        # Enviar el mensaje "change" seguido de un salto de línea
        uart.write("change\n")
        # Pequeña espera para evitar múltiples envíos no intencionados (debounce)
        sleep(200)
```

## Proceso de conexión y comunicación
### Conexión física:

* Micro:bit: Conecta el micro:bit a tu computadora mediante un cable USB.
* p5.js: Abre tu sketch de p5.js en un navegador compatible con la API Web Serial (por ejemplo, Google Chrome o Microsoft Edge).
### Carga de programas:

* Carga el código en el micro:bit usando el entorno de desarrollo que prefieras (por ejemplo, el editor de MicroPython o MakeCode) y flashea el dispositivo.
* Ejecuta el programa p5.js. Es posible que necesites interactuar (por ejemplo, haciendo clic en la pantalla o en un botón de conexión) para permitir que el navegador acceda al puerto serial.
### Establecimiento de la comunicación:

* El programa p5.js utiliza la librería p5.webserial para solicitar acceso al puerto serial y establecer la comunicación con el micro:bit.
* Una vez establecida la conexión, la función gotData se activará cada vez que se reciba una línea de datos del micro:bit.
### Interacción:

* Al presionar el botón A en el micro:bit, el código en MicroPython detecta el evento y envía la cadena "change\n" a través del puerto serial.
El programa en p5.js recibe el mensaje, lo procesa y, al coincidir con el comando "change", cambia el color del cuadrado a un color aleatorio.
