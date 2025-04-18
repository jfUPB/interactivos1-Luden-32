### Paso 15: Presionar los botones A y B

#### Descripción: 
Al presionar los botones A y B del micro:bit, la pantalla del editor de p5.js muestra la letra correspondiente al botón presionado. Además, el círculo en la pantalla cambia de color: rojo para A y amarillo para B.

#### Cómo se logra: 
El micro:bit detecta la presión del botón y envía un carácter a la computadora mediante UART. En p5.js, el puerto serial lee esta información y cambia la interfaz gráfica de acuerdo con los datos recibidos.

### Paso 16: Sacudir el dispositivo

#### Descripción: 
Al agitar el micro:bit, en la pantalla del editor de p5.js aparece la letra 'C' y el círculo se vuelve verde.

#### Cómo se logra: 
Cuando se reconoce el gesto, el código de micro:bit envía la letra 'C' por la conexión serial. En p5.js, la interfaz lee esta entrada y cambia la visualización. 

### Paso 17: Presionar el botón "Send Love"

#### Descripción: 
Al hacer clic en el botón "Send Love" dentro de la interfaz de p5.js, la pantalla del micro:bit muestra un corazón por un momento y luego cambia a una carita feliz.

#### Cómo se logra: 
Cuando se presiona el botón en p5.js, se ejecuta la función sendBtnClick(), que envía el carácter 'h' al micro:bit. Este, al recibir el dato, cambia la visualización en su pantalla de acuerdo con la lógica definida en su código.

