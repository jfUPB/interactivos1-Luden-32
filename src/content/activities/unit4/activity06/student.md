**¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?**

Es un conjunto de reglas que define cómo se envían y reciben datos. Es importante porque permite que los dispositivos se comuniquen de manera clara y sin errores.

**¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?**

Para facilitar la separación y lectura de los datos en el programa receptor.

**¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?**

Para que el receptor sepa cuándo ha terminado de recibir un mensaje completo y pueda procesarlo correctamente.

**¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?**

Para organizar el flujo del programa, asegurando que solo ejecute ciertas acciones cuando el micro:bit esté conectado y en el estado adecuado.

**¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?**

Se convierten en una cadena con valores separados por comas y se finalizan con un salto de línea (`\n`).

**¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?**

Que los valores se representan como texto en formato ASCII, lo que facilita su interpretación en distintos dispositivos.

**¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?**

Para evitar errores al intentar leer datos cuando el buffer está vacío.

**¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?**

Usando `.trim()` para eliminar espacios y caracteres de nueva línea al inicio y al final de la cadena.

**Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales, ¿qué función de p5.js usarías?**

La función `.split(" ")`.

**¿Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?**

Porque los valores recibidos son texto y deben convertirse en números para realizar cálculos y manipular la posición en pantalla.

**¿Qué aprendiste del micro:bit que no sabías antes?**

Cómo enviar datos a través del puerto serial y comunicarse con una computadora en tiempo real.

**¿Qué aprendiste nuevo de p5.js que no sabías antes?**

Cómo manejar la comunicación serial con `createSerial()` y procesar datos del micro:bit en un sketch interactivo.
