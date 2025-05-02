# Probando y Depurando la Bomba Temporizada (versión “scroll-decrement”)

## 1. Vectores de Prueba

| #  | Estado Inicial           | Evento(s)                             | Resultado Esperado                                                                                   |
|----|--------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------------|
| 1  | **CONFIG**, time_left=20 | — (sin interacción)                   | Sigue en CONFIG, muestra “20” en scroll cada ciclo, no cambia de estado.                             |
| 2  | **CONFIG**, time_left=20 | button_a.was_pressed()                | `time_left` → 21, el siguiente scroll muestra “21”.                                                   |
| 3  | **CONFIG**, time_left=21 | button_b.was_pressed()                | `time_left` → 20, el siguiente scroll muestra “20”.                                                   |
| 4  | **CONFIG**, time_left=5  | accelerometer.was_gesture("shake")    | Transición a ARMED; `display.show(Image.YES)`; luego va a COUNTDOWN.                                   |
| 5  | **COUNTDOWN**, time_left=3| — (esperar ciclo completo de scroll)  | Scroll “3” → al terminar `time_left`=2; siguiente scroll “2” → luego `time_left`=1.                    |
| 6  | **COUNTDOWN**, time_left=4| pin_logo.is_touched() antes de scroll | Transición inmediata a CONFIG; se conserva `time_left` (4).                                            |
| 7  | **COUNTDOWN**, time_left=1| — (esperar scroll de “1”)             | Scroll “1” → `time_left`=0 → Estado pasa a EXPLODED; muestra `Image.SKULL`; suena alarma.             |
| 8  | **EXPLODED**             | pin_logo.is_touched()                 | Transición a CONFIG; `time_left` restablecido a 20; vuelve a modo CONFIG.                             |

## 2. Errores Encontrados y Correcciones

1. **Conteo basado en utime vs scroll**  
   - *Síntoma:* La lógica con `ticks_ms()` resultaba imprecisa al combinarla con `display.scroll()`, y el scroll bloqueaba la detección de eventos.  
   - *Corrección:* Simplifiqué el COUNTDOWN para que, tras cada `display.scroll(str(time_left))`, se decremente `time_left` en 1. Así el fin del scroll define el fin de cada segundo.

2. **Bloqueo de eventos durante el scroll**  
   - *Síntoma:* Mientras el scroll estaba activo no se podía detectar `pin_logo.is_touched()`.  
   - *Corrección:* Antes de hacer el scroll comprobamos `pin_logo.is_touched()` y, solo si no hay toque, procedemos al scroll y decremento.

3. **Transición confusa al explotar**  
   - *Síntoma:* La condición `time_left <= 0` no se evaluaba justo al terminar el scroll de “1”.  
   - *Corrección:* Tras el decremento, evaluamos inmediatamente `if time_left <= 0: current_state = STATE_EXPLODED`.

4. **Reinicio no conservaba ajuste en COUNTDOWN**  
   - *Síntoma:* Al reiniciar tocando `pin_logo` en COUNTDOWN, el tiempo volvía a 20 aunque el usuario lo hubiera ajustado.  
   - *Corrección:* Ahora, en COUNTDOWN la transición a CONFIG conserva el valor actual de `time_left`; solo en EXPLODED se restablece a 20.

5. **Duplicación de lógica de estado**  
   - *Síntoma:* Existían múltiples asignaciones de `current_state` a COUNTDOWN dentro de ARMED.  
   - *Corrección:* Simplifiqué ARMED para hacer un único cambio de estado tras la confirmación y la pausa.

## 3. Código Final Corregido

```python
from microbit import *
import music

# Estados
STATE_CONFIG    = 0
STATE_ARMED     = 1
STATE_COUNTDOWN = 2
STATE_EXPLODED  = 3

# Variables de control
current_state = STATE_CONFIG
time_left     = 20  # valor inicial (ajustable)

while True:
    if current_state == STATE_CONFIG:
        # Mostrar tiempo configurado
        display.scroll(str(time_left))
        # Ajustar con A/B
        if button_a.was_pressed() and time_left < 60:
            time_left += 1
        if button_b.was_pressed() and time_left > 10:
            time_left -= 1
        # Armar al agitar
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED

    elif current_state == STATE_ARMED:
        # Confirmación de armado
        display.show(Image.YES)
        sleep(1000)  # 1 s antes de iniciar
        current_state = STATE_COUNTDOWN

    elif current_state == STATE_COUNTDOWN:
        # Si tocan logo, reiniciar
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
        else:
            # Mostrar el tiempo actual y luego decrementarlo
            display.scroll(str(time_left))
            time_left -= 1
            # Si llega a cero, explota
            if time_left <= 0:
                current_state = STATE_EXPLODED

    elif current_state == STATE_EXPLODED:
        # Explosión
        display.show(Image.SKULL)
        music.play(music.WAWAWAWAA)
        # Reinicio tras toque
        if pin_logo.is_touched():
            time_left = 20
            current_state = STATE_CONFIG
