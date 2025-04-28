```py
from microbit import *
import music
import utime

# Estados
STATE_CONFIG    = 0
STATE_ARMED     = 1
STATE_COUNTDOWN = 2
STATE_EXPLODED  = 3

# Variables globales
current_state   = STATE_CONFIG
time_left       = 20
deact_step      = 0
event_occurred  = False
current_event   = None
prev_touch      = False  # Para detección de flanco en touch
armed_start_time = 0     # Tiempo de inicio en estado ARMADO

# Inicializar UART para comunicación serial
uart.init(baudrate=9600, tx=pin1, rx=pin0)

def tareaBomba():
    global current_state, time_left, deact_step, event_occurred, current_event, armed_start_time

    if current_state == STATE_CONFIG:
        display.scroll(str(time_left))
        if event_occurred:
            if current_event == 'A' and time_left < 60:
                time_left += 1
            elif current_event == 'B' and time_left > 10:
                time_left -= 1
            elif current_event == 'S':  # Shake para armar
                current_state = STATE_ARMED
                armed_start_time = utime.ticks_ms()
            event_occurred = False  # Consumir evento

    elif current_state == STATE_ARMED:
        display.show(Image.YES)
        # Esperar 1s sin bloquear usando tiempo diferencial
        if utime.ticks_diff(utime.ticks_ms(), armed_start_time) >= 1000:
            current_state = STATE_COUNTDOWN
            deact_step = 0

    elif current_state == STATE_COUNTDOWN:
        if event_occurred:
            # Verificar secuencia de desactivación A-B-A-S
            if deact_step == 0 and current_event == 'A':
                deact_step = 1
            elif deact_step == 1 and current_event == 'B':
                deact_step = 2
            elif deact_step == 2 and current_event == 'A':
                deact_step = 3
            elif deact_step == 3 and current_event == 'S':
                time_left = 20
                current_state = STATE_CONFIG  # Desactivación exitosa
            else:  # Cualquier evento incorrecto
                deact_step = 0
            event_occurred = False  # Consumir evento

        # Reinicio rápido con touch (evento 'T')
        if event_occurred and current_event == 'T':
            current_state = STATE_CONFIG
            event_occurred = False

        # Actualizar cuenta regresiva
        display.scroll(str(time_left))
        time_left -= 1
        if time_left <= 0:
            current_state = STATE_EXPLODED

    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        music.play(music.WAWAWAWAA)
        if event_occurred and current_event == 'T':  # Reiniciar con touch
            time_left = 20
            current_state = STATE_CONFIG
            event_occurred = False

def tareaEventos():
    global event_occurred, current_event, prev_touch

    # 1. Detectar touch (flanco ascendente)
    current_touch = pin_logo.is_touched()
    if current_touch and not prev_touch:
        current_event = 'T'
        event_occurred = True
    prev_touch = current_touch  # Actualizar estado anterior

    # 2. Detectar botones A/B y shake
    if button_a.was_pressed():
        current_event = 'A'
        event_occurred = True
    if button_b.was_pressed():
        current_event = 'B'
        event_occurred = True
    if accelerometer.was_gesture("shake"):
        current_event = 'S'
        event_occurred = True

    # 3. Leer puerto serial (eventos externos)
    if uart.any():
        msg = uart.readline().decode().strip().upper()  # Ej: 'A' → 'A'
        if msg in ['A', 'B', 'S', 'T']:  # Filtrar comandos válidos
            current_event = msg
            event_occurred = True

# Bucle principal no bloqueante
while True:
    tareaBomba()
    tareaEventos()
    sleep(100)  # Reducir parpadeo
```
**Explicación de la Implementación:**

1. **Variables de Eventos Unificadas:**
    - **`event_occurred`**: Booleano que indica si hay un evento pendiente de procesar.
    - **`current_event`**: Almacena el tipo de evento (**`A`**, **`B`**, **`S`**, **`T`**), independientemente de su origen (físico o serial).
2. **Separación de Tareas:**
    - **`tareaEventos()`:** Centraliza la detección de eventos:
        - **Touch:** Usa detección de flanco para generar un solo evento **`T`** al tocar.
        - **Botones y Shake:** Usa **`was_pressed()`** y **`was_gesture()`** para eventos de pulsación única.
        - **Serial:** Lee mensajes y los mapea a eventos válidos (ej: **`'A'`** → evento **`A`**).
    - **`tareaBomba()`:** Procesa los eventos almacenados:
        - Consume **`event_occurred`** tras procesar cada evento.
        - La lógica de estados **no accede directamente a sensores**, solo usa los eventos.
3. **Máquina de Estados Mejorada:**
    - **No bloqueante:** Usa **`utime.ticks_ms()`** para transiciones temporizadas (ej: espera de 1s en **`ARMADO`**).
    - **Secuencia de Desactivación:** Utiliza **`current_event`** en lugar de leer botones directamente, permitiendo que la secuencia **`A-B-A-S`** se ingrese tanto físicamente como por serial.
4. **Manejo de Serial:**
    - **Formato de Mensajes:** Los mensajes seriales deben ser strings (**`'A'`**, **`'B'`**, etc.).
    - **Sanitización:** Se eliminan espacios y se convierte a mayúsculas para evitar errores.

**Ejemplo de Uso con Serial:**

1. Enviar **`A`** por serial incrementa el tiempo en **`STATE_CONFIG`**.
2. Enviar **`S`** por serial arma la bomba (equivalente a agitarla).
3. Durante **`COUNTDOWN`**, enviar **`A→B→A→S`** por serial desactiva la bomba.

**2 / 2**
