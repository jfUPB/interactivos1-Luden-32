### Comparación entre protocolo ASCII y protocolo binario

| **Aspecto** | **Protocolo ASCII** | **Protocolo Binario** |
| --- | --- | --- |
| **Eficiencia** | Es menos eficiente, ya que representa los datos como caracteres de texto, lo que implica utilizar más bytes y, por lo tanto, más recursos. | Resulta más eficiente al permitir empaquetar los datos en una cantidad menor de bytes. Por ejemplo, un entero de 16 bits puede ocupar solo 2 bytes. |
| **Velocidad** | La transmisión tiende a ser más lenta debido al volumen de caracteres que se deben enviar. | Al requerir menos espacio, los datos binarios se procesan y transmiten más rápido. |
| **Facilidad de uso** | Más sencillo de entender y programar, ya que utiliza texto legible por humanos. | Su implementación es más compleja porque requiere codificar y decodificar los datos, y no es directamente comprensible para una persona. |
| **Uso de recursos** | Demanda más memoria y ancho de banda, ya que los datos ocupan más espacio. | Optimiza el uso de recursos, gracias a que los datos ocupan menos espacio al estar codificados de forma más compacta. |

### Ejemplo en una aplicación

Cuando se emplea el protocolo ASCII, los valores como `xValue`, `yValue`, o los estados de botones (`aState`, `bState`) se convierten en texto, lo cual consume más bytes por dato.

En cambio, con el protocolo binario, se pueden empaquetar estos datos directamente con funciones como `struct.pack()`, lo que mejora tanto la eficiencia de la memoria como la velocidad de transmisión.

---

### Framing en el protocolo binario

**¿Por qué es necesario?**

El *framing* permite delimitar con claridad dónde comienza y termina cada paquete de datos. Sin él, los datos se mezclarían, impidiendo su correcta interpretación.

**¿Cómo funciona?**

Se incluye un byte especial o un encabezado que actúa como marcador de inicio. En el caso del micro:bit, se utiliza el valor `0xAA` para indicar el comienzo de un nuevo paquete.

---

### Otros conceptos importantes

**Carácter de sincronización**

Es un valor específico que indica cuándo comienza un paquete de datos. `0xAA` actúa como este carácter, sirviendo para alinear correctamente la lectura.

**Checksum**

Es una verificación que se realiza para asegurar que los datos no hayan sufrido alteraciones durante la transmisión. Se calcula sumando todos los bytes y tomando el resultado módulo 256. Al llegar al destino, se recalcula y se compara con el original.

---

### En el código de la función `readSerialData()` en p5.js:

- **`concat()`**: Une los nuevos datos recibidos al buffer existente, asegurando que no se pierda información entre lecturas.
- **Revisión del tamaño del buffer (≥8 bytes)**: Se comprueba que haya al menos 8 bytes antes de procesar, ya que ese es el tamaño mínimo del paquete.
- **`0xAA`**: Marca el comienzo de un paquete. Si un byte no coincide con este valor, se descarta.
- **`shift()` y `continue`**: `shift()` elimina el primer byte del buffer si no es válido, y `continue` salta a la siguiente iteración del bucle sin intentar procesarlo.
- **`break` si hay menos de 8 bytes**: Se sale del bucle para esperar más datos, evitando errores por intentar leer paquetes incompletos.
- **`slice()` vs `splice()`**: `slice()` copia una parte del buffer sin modificarlo; `splice()` elimina esos bytes ya procesados del buffer original.
- **`reduce()`**: Se usa para sumar los bytes y obtener el *checksum*.
    
    ```jsx
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
    ```
    
- **Comparación del *checksum***: Verifica que los datos lleguen intactos. Si no coinciden, el paquete se considera corrupto y se descarta con `continue`.

---

### ¿Qué es un `DataView`?

Es una herramienta que permite leer bytes desde un buffer como diferentes tipos de datos (enteros, flotantes, etc.).

Por ejemplo:

```jsx
let buffer = new Uint8Array(dataBytes).buffer;
let view = new DataView(buffer);
```

Esto es fundamental para interpretar correctamente los bytes binarios, ya que no se puede asumir directamente qué tipo de dato representan sin esta conversión.
