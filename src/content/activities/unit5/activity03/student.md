### **¿Por qué antes necesitábamos delimitadores y ahora no? ¿Y por qué ahora no se necesita?**

En la unidad anterior, los datos se enviaban como texto en ASCII (`"{},{},{},{}\n"`). En este formato, los valores pueden tener tamaños variables, por lo que era necesario:

- Separar los valores con comas `,` para saber dónde empieza y termina cada dato.
- Agregar un salto de línea (`\n`) para marcar el final de cada paquete y que el receptor pueda reconocer cuándo comienza uno nuevo.

Ahora, usamos formato binario con `struct.pack('>2h2B')`, que tiene un tamaño fijo de 6 bytes por paquete.

### Cambio en el Formato de Datos Enviados por el micro:bit

**Unidad Anterior: Datos en Texto con Separadores**

Antes, los datos se enviaban como una cadena de texto separada por comas y finalizada con un salto de línea (`\n`).

**Nueva Unidad: Datos en Binario de Tamaño Fijo**

Ahora, en vez de enviar texto, los datos se envían en binario con un formato fijo.

### Cambio en la Recepción de Datos en Processing

**Unidad Anterior: Procesamiento Basado en Texto**

Antes, en Processing se leía una línea de texto completa.

**Nueva Unidad: Procesamiento Binario Estructurado**

Ahora, se leen exactamente 6 bytes.

### **¿Qué ves en la consola? ¿Por qué crees que se produce este error?**

```
microBitX: 1024 microBitY: -500 microBitAState: true microBitBState: false
microBitX: 32767 microBitY: -1 microBitAState: false microBitBState: true
microBitX: -500 microBitY: 256 microBitAState: false microBitBState: false
```

El problema ocurre porque la comunicación serial no garantiza que los datos lleguen siempre en paquetes alineados de 6 bytes exactos. Puede pasar que:

- **Lectura desincronizada**: Si los datos llegan en partes y `port.readBytes(6)` no comienza a leer exactamente al inicio de un paquete válido, se pueden interpretar bytes de diferentes paquetes como si fueran un solo mensaje.
- **Pérdida de datos**: Si se pierden bytes en la transmisión, el código leerá un paquete incompleto, lo que puede generar valores incorrectos.
- **Lecturas desfasadas**: Si un byte extra o faltante se introduce en la secuencia, toda la lectura se desajusta, haciendo que los valores obtenidos sean incorrectos.

```
microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false
```

### Cambios en los programas:

**Código del micro:bit:**

- **Uso del acelerómetro**:
    
    Se reemplazaron los valores fijos de `xValue` y `yValue` por los valores del acelerómetro con `accelerometer.get_x()` y `accelerometer.get_y()`.
    
    Esto hace que el dibujo en p5.js ahora reaccione a la inclinación del micro:bit.
    
- **Uso real de botones**:
    
    Se reemplazaron los valores fijos de `aState` y `bState` por la lectura real de los botones A y B con `button_a.is_pressed()` y `button_b.is_pressed()`.
    

**Código de p5.js:**

- **Limpieza de la consola**:
    
    Se eliminó el `console.log()` de la función `readSerialData()`, lo que puede hacer que la consola se vea menos saturada con mensajes.
    

**¿Qué ves?**

```
Microbit ready to draw
A pressed
B released
Checksum error in packet
```
