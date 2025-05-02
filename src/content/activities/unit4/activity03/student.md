### **Análisis del código del micro:bit**

Este programa permite transmitir datos desde un micro:bit mediante comunicación serial UART. A continuación, se desglosa su funcionamiento:

```python
from microbit import *  # Carga todas las funcionalidades del dispositivo

uart.init(115200)  # Configura la comunicación serial a 115200 baudios
display.set_pixel(0, 0, 9)  # Activa el LED en la esquina superior izquierda

while True:
    xValue = accelerometer.get_x()  # Obtiene la aceleración en el eje X
    yValue = accelerometer.get_y()  # Obtiene la aceleración en el eje Y
    aState = button_a.is_pressed()  # Detecta si el botón A está presionado
    bState = button_b.is_pressed()  # Detecta si el botón B está presionado
    data = f"{xValue},{yValue},{aState},{bState}\n"  # Formatea los datos
    uart.write(data)  # Transmite por el puerto serial
    sleep(100)  # Espera 100 ms antes de repetir (10 transmisiones/segundo)
```

---

### **Datos transmitidos**

El micro:bit envía cuatro valores en tiempo real:

1. **xValue**: Aceleración en el eje X (rango: -1024 a 1024).
2. **yValue**: Aceleración en el eje Y (rango: -1024 a 1024).
3. **aState**: `True` si el botón A está presionado, `False` en caso contrario.
4. **bState**: `True` si el botón B está presionado, `False` en caso contrario.

---

### **Estructura de los datos**

- **Formato**: Los valores se concatenan como texto, separados por comas y terminados con `\n`.
    
    Ejemplo: `"969,652,True,False\n"`.
    
- **Propósito de las comas**: Facilitan la división de valores en el receptor (p. ej., usando `split(",")`).
- **Salto de línea (`\n`)**: Marca el final de cada paquete, permitiendo al receptor identificar mensajes completos.

---

### **Función del `sleep(100)`**

- Limita la frecuencia de transmisión a **10 Hz** (10 paquetes/segundo).
- Sin esta pausa, el micro:bit saturaría el puerto serial, causando pérdida de datos o errores en el receptor.

---

### **Interpretación de valores**

- **Acelerómetro**:
    - **Eje X**:
        - Valor negativo → Inclinación a la izquierda.
        - Valor positivo → Inclinación a la derecha.
    - **Eje Y**:
        - Valor negativo → Inclinación hacia adelante.
        - Valor positivo → Inclinación hacia atrás.
- **Botones**:
    - `is_pressed()`: Detecta si el botón está presionado **en el instante actual**.
    - `was_pressed()`: Detecta si el botón se pulsó **en la iteración anterior** (no usado en este código).

---

### **Transmisión de bytes**

Cada paquete (ejemplo: `"969,652,True,False\n"`) se envía como una secuencia de caracteres ASCII. Por ejemplo:

- `9` → hexadecimal `0x39`
- `,` → hexadecimal `0x2C`
- `T` (de `True`) → hexadecimal `0x54`
- `\n` → hexadecimal `0x0A`
