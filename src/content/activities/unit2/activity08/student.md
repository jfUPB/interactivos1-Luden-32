### **1. Estados Identificados**

Los estados son momentos en los que el programa espera a que ocurra algo. En el código, se definen dos estados explícitos en la clase `Pixel`:

- **`"Init"`**:
    - Estado inicial del pixel.
    - **Propósito**: Configurar el tiempo inicial (`startTime`) y encender/apagar el pixel en la matriz LED según `pixelState`.
    - **Transición**: Inmediatamente cambia a `"WaitTimeout"` después de inicializar.
- **`"WaitTimeout"`**:
    - Estado de espera activa.
    - **Propósito**: Esperar a que transcurra el intervalo de tiempo definido (`interval`).
    - **Transición**: Permanece en este estado hasta que se cumple el intervalo, momento en el que se ejecutan acciones (togglear el pixel) y se reinicia el ciclo.

---

### **2. Eventos Identificados**

Los eventos son condiciones que el programa verifica durante un estado. En este caso:

- **Evento de Tiempo (Timeout)**:
    - **Descripción**: Ocurre cuando la diferencia entre el tiempo actual (`utime.ticks_ms()`) y el `startTime` supera el `interval`.
    - **Cómo se verifica**:
        
        python
        
        Copy
        
        ```
        utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval
        ```
        
    - **Relevancia**: Es el único evento que desencadena acciones en el estado `"WaitTimeout"`.

---

### **3. Acciones Identificadas**

Las acciones son operaciones ejecutadas en respuesta a eventos o transiciones de estado:

- **En el estado `"Init"`**:
    - **Acción 1**: Inicializar `startTime` con el tiempo actual.
    - **Acción 2**: Cambiar al estado `"WaitTimeout"`.
    - **Acción 3**: Actualizar el estado del pixel en la matriz LED (`display.set_pixel()`).
- **En el estado `"WaitTimeout"` (cuando ocurre el timeout)**:
    - **Acción 1**: Reiniciar `startTime` con el tiempo actual.
    - **Acción 2**: Togglear el `pixelState` (alternar entre 0 y 9, que representan apagado y encendido).
    - **Acción 3**: Actualizar el pixel en la matriz LED con el nuevo estado.
