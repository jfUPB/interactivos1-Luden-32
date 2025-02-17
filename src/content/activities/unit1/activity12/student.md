## p5.js
```js

let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');   
        }
        else if(dataRx == 'B'){
            fill('yellow'); 
        }
        else{
            fill('green'); 
        }
        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }    


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } 
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
}
```

## micro:bit 
```python
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
        display.show(Image.SILLY)
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        display.show(Image.PACMAN)
        sleep(500)
    if accelerometer.was_gesture('shake'):
        uart.write('C')
        display.show(Image.GHOST)
        sleep(500)
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(9000)
                display.show(Image.SKULL)
```
## Explicación
Para este ejercicio modifiqué algunas acciones que se realizaban en el micro:bit, enfocándome única y exclusivamente en su programa. Es decir, al tener ambos programas conectados, hice que, cuando se reciba la señal del botón A, en lugar de simplemente indicar que se presionó dicho botón, se mostrara una imagen del repositorio de imágenes disponibles. Realicé lo mismo con el botón B y al agitar el micro:bit. Además, dejé el corazón en pantalla por más tiempo, modificando el "sleep" de sleep(500) a sleep(9000), para que así permaneciera visible durante 9 segundos.
