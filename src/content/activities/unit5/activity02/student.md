### **1. Visualización de Datos en Modo Texto**

**Experimento**:

- **Configuración**: SerialTerminal en modo "Texto".
- **Resultado**: Caracteres ilegibles como **`�`**, **`~`**, o símbolos sin sentido.
- **Análisis**:
    
    Los bytes binarios no representan caracteres ASCII válidos. Por ejemplo, el byte **`0xFF`** corresponde al carácter ÿ en ASCII extendido, pero valores fuera del rango 0-127 se interpretan como caracteres inválidos.
    

---

### **2. Visualización en Hexadecimal**

**Experimento**:

- **Configuración**: SerialTerminal en modo "Todo en Hex".
- **Resultado**: Secuencias como **`FF 9C 01 00 00 01`**.
- **Relación con el código**:
    
    El formato **`>2h2B`** empaqueta:
    
    - **2h**: Dos enteros cortos (16 bits cada uno, big-endian).
        
        Ejemplo: **`xValue = -100`** → **`0xFF9C`** (complemento a 2).
        
    - **2B**: Dos bytes sin signo (0 o 1 para los botones).
        
        Ejemplo: **`aState=0`**, **`bState=1`** → **`00 01`**.
        

---

### **3. Ventajas y Desventajas del Formato Binario**

| **Binario** | **ASCII** |
| --- | --- |
| ✅ **Compacto**: 6 bytes/mensaje | ❌ **Grande**: ~20 bytes/mensaje |
| ✅ **Rápido procesamiento** | ❌ **Lento por conversión** |
| ❌ **Difícil depuración** | ✅ **Legible para humanos** |

---

### **4. Experimento con Gestos "Shake"**

**Código modificado**:

```py
if accelerometer.was_gesture('shake'):
    # Empaquetar y enviar datos
```

**Resultado**:

- **Bytes por mensaje**: 6 (2 enteros cortos * 2 bytes + 2 bytes booleanos).
- **Desglose**:
    - Bytes 1-2: **`xValue`** (ej: **`0x03E8`** = 1000).
    - Bytes 3-4: **`yValue`** (ej: **`0xFE0C`** = -500).
    - Byte 5: **`aState`** (**`0x00`** o **`0x01`**).
    - Byte 6: **`bState`** (**`0x00`** o **`0x01`**).

---

### **5. Representación de Números Negativos**

- **Formato**: Complemento a 2.
- **Ejemplo**:
    - **`xValue = -100`** → **`0xFF9C`** en hexadecimal.
    - **`yValue = -255`** → **`0xFF01`**.

---

### **6. Comparación ASCII vs Binario**

**Experimento**:

```py
uart.write(data)          # Binario (6 bytes)
uart.write("ASCII:\n")    # Texto
uart.write(data)          # ASCII (ej: "-100,255,True,False\n" → 20 bytes)
```

**Diferencias**:

| **Binario** | **ASCII** |
| --- | --- |
| **`FF9C FF01 00 01`** | **`-100,255,True,False\n`** |
| 6 bytes | ~20 bytes |

**Ventajas/Desventajas**:

- **Binario**: Ideal para alta frecuencia de datos (ej: 10 Hz).
- **ASCII**: Útil para depuración sin herramientas especializadas.
