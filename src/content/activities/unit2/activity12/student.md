```py
from microbit import *
import utime
import music

# Definir estados de la máquina
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_COUNTDOWN = 2
STATE_EXPLODED = 3

# Variables de control
current_state = STATE_CONFIG
time_left = 20  # Tiempo inicial en segundos (valor configurable)
start_time = 0

while True:
    if current_state == STATE_CONFIG:
        # Mostrar el tiempo programado en la pantalla
        display.scroll(str(time_left))
        
        # Ajustar el tiempo con los botones UP (A) y DOWN (B)
        if button_a.was_pressed() and time_left < 60:
            time_left += 1
        if button_b.was_pressed() and time_left > 10:
            time_left -= 1
        
        # Activar la bomba con el gesto shake (ARMED)
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED

    elif current_state == STATE_ARMED:
        # Mostrar confirmación de armado
        display.show(Image.YES)
        # Guardar el tiempo inicial de la cuenta regresiva
        start_time = utime.ticks_ms()
        current_state = STATE_COUNTDOWN

    elif current_state == STATE_COUNTDOWN:
        # Calcular el tiempo transcurrido y determinar el tiempo restante
        elapsed_time = (utime.ticks_ms() - start_time) // 1000
        remaining_time = time_left - elapsed_time
        
        if remaining_time > 0:
            display.scroll(str(remaining_time))
        else:
            current_state = STATE_EXPLODED  # Se activa la detonación cuando el tiempo llega a 0
        
        # Permitir reiniciar la bomba a modo de configuración si se toca el sensor touch (pin_logo)
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            time_left = 20

    elif current_state == STATE_EXPLODED:
        # Mostrar imagen de explosión y reproducir la alarma
        display.show(Image.SKULL)
        music.play(music.WAWAWAWAA)
        
        # Reiniciar el sistema al detectar un toque en el sensor touch
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            time_left = 20
```
