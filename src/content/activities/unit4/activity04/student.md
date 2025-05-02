### **Compara el código original del caso de estudio con el anterior. ¿Qué notas de diferente?**

 Al comparar el código original del micro:bit en Python con la estructura del nuevo proyecto en p5.js, se observan varias diferencias significativas:

- El código en Python se ejecuta directamente en el micro:bit y recoge datos de sensores (acelerómetro, botones) para enviarlos por el puerto serial usando `uart.write()`.
- El proyecto en p5.js corre en un navegador web, usando JavaScript con las librerías p5.js (visualización) y p5.webserial.js (comunicación serial).
- El código Python se enfoca en recolectar y transmitir datos, mientras que p5.js prioriza la recepción y representación visual.
- El código HTML en p5.js vincula archivos necesarios para la conexión, algo inexistente en el código del micro:bit.

### **Ejecuta el sketch. Reflexiona: ¿Para qué se usan estas imágenes? ¿Qué representan?**

Las imágenes SVG (03.svg, 04.svg, etc.) funcionan como un sistema de estados visuales:

- Representan gráficamente los datos del sensor en tiempo real (ej: inclinación = imagen 03.svg, botón presionado = imagen 04.svg).
- Se precargan en `preload()` para garantizar su disponibilidad durante la visualización.

### **Bitácora: Máquina de estados**

- **Estados principales**:
    - `WAIT_MICROBIT_CONNECTION`: Espera conexión del micro:bit.
    - `RUNNING`: Micro:bit conectado y datos recibidos.
- **Función `setup()`** actúa como pseudoestado `INIT` (prepara la aplicación una vez al inicio).

### **¿Qué pasaría si das click al botón "Connect to micro:bit"?**

- **Proceso**:
    1. Ejecuta `connectBtnClick()` (vinculado a `mousePressed()`).
    2. Si el puerto **no está abierto**:
        - Abre conexión serial (`port.open("MicroPython", 115200)`).
    3. Si **ya está abierto**: Cierra la conexión (`port.close()`).

### **Nota sobre lectura de datos**

- **¿Por qué solo se leen datos con el puerto abierto?**
    - Un puerto cerrado no permite comunicación. El micro:bit envía datos "al vacío" si no hay receptor.

### **Protocolo de datos del micro:bit**

- **Formato**: Valores separados por comas y terminados en `\n` (ej: `x,y,A,B\n`).
- **Procesamiento en p5.js**:
    - Verifica datos disponibles (`port.availableBytes() > 0`).
    - Lee hasta `\n` (`port.readUntil("\n")`).
    - Separa valores (`split(",")`) y valida integridad (`values.length == 4`).
    - Convierte tipos: `x`/`y` a enteros, botones a booleanos.
    - Ajusta coordenadas al centro de la pantalla (`windowWidth/2 + x`, `windowHeight/2 + y`).

### **Función `updateButtonStates`**

- Detecta cambios en botones A/B del micro:bit para simular eventos de teclado:
    - Compara estados actuales y anteriores (`prevmicroBitAState`, `prevmicroBitBState`).
    - Dispara acciones solo en transiciones (ej: de "no presionado" a "presionado").

### **Diferencias clave con el código original**

- **Flujo controlado por estados**:
    - Evita dibujos sin datos válidos (espera conexión explícita).
    - Elimina interacciones locales (clics/movimiento de mouse) para depender totalmente del micro:bit.
- **Botones del micro:bit** reemplazan funcionalidades previas (ej: barra espaciadora).

### **Error: "No se están recibiendo 4 datos del micro:bit"**

- **Causas**:
    - Datos incompletos o mal formateados (falta de `\n`, interferencias).
- **Comportamiento**:
    - Mensaje aparece en frames con datos incorrectos.
- **Soluciones**:
    - Asegurar envío correcto desde micro:bit (4 valores + `\n`).
    - Mejorar manejo de errores (reintentos, depuración de datos recibidos).
    - Sincronizar lectura (esperar paquetes completos).
