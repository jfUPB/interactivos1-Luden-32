### **Respuestas al Caso de Estudio: Comunicación entre micro:bit y p5.js**

### **1. Comunicación y Datos Enviados por el micro:bit**

El micro:bit envía **4 datos** a través del puerto serial en formato ASCII:

```python
data = "{},{},{},{}\\n".format(xValue, yValue, aState, bState)

```

- **xValue**: Valor del acelerómetro en el eje X (rango: -1024 a 1024).
- **yValue**: Valor del acelerómetro en el eje Y (rango: -1024 a 1024).
- **aState**: Estado del botón A (`True`/`False`).
- **bState**: Estado del botón B (`True`/`False`).

---

### **2. Estructura del Protocolo ASCII**

El protocolo sigue este formato:

```
<valor_x>,<valor_y>,<estado_A>,<estado_B>\\n

```

Ejemplo de un mensaje:

```
-512,300,True,False\\n

```

- **Delimitadores**: Comas (`,`) separan los valores.
- **Terminación**: Salto de línea (`\\n`) indica fin de mensaje.
- **Tipos de datos**:
    - `xValue` e `yValue` son enteros.
    - `aState` y `bState` son booleanos (`True`/`False`).

---

### **3. Lectura y Transformación de Datos en p5.js**

En el sketch, esta sección procesa los datos:

```jsx
// En la función draw():
if (port.availableBytes() > 0) {
  let data = port.readUntil("\\n").trim();
  if (data) {
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;  // Mapeo a centro horizontal
      microBitY = int(values[1]) + windowHeight / 2; // Mapeo a centro vertical
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}

```

- **Mapeo de coordenadas**:
    - Los valores del acelerómetro se centran en la pantalla sumando `windowWidth/2` y `windowHeight/2`.
    - Ejemplo: Si `xValue = -512` y el ancho es 720, `microBitX = -512 + 360 = -152`.

---

### **4. Generación de Eventos "A pressed" y "B released"**

La función `updateButtonStates()` detecta flancos en los botones:

```jsx
function updateButtonStates(newAState, newBState) {
  // Detección de flanco ascendente en A (A pressed)
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160); // Cambia tamaño de línea
    clickPosX = microBitX;            // Guarda posición inicial
    print("A pressed");
  }

  // Detección de flanco descendente en B (B released)
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100)); // Cambia color
    print("B released");
  }

  // Actualiza estados anteriores
  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

```

- **A pressed**: Se detecta cuando `aState` cambia de `False` → `True`.
- **B released**: Se detecta cuando `bState` cambia de `True` → `False`.

---

### **5. Capturas de Ejemplo**

*(Descripciones ilustrativas, ya que no se incluyen imágenes reales)*

| **Interacción** | **Efecto Visual** |
| --- | --- |
| **Mover micro:bit** | Líneas que siguen el movimiento del dispositivo (ej: espirales o figuras onduladas). |
| **Presionar A + Inclinación** | Líneas gruesas con colores cálidos (ej: naranjas y amarillos). |
| **Soltar B + Rotación** | Cambio abrupto a tonos fríos (ej: azules y verdes) con transparencia. |
| **Combinar Shift + Movimiento** | Líneas restringidas a ejes (horizontales/verticales) para patrones geométricos. |
