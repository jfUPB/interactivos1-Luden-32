```py
from microbit import *
import music

# Estados
STATE_CONFIG    = 0
STATE_ARMED     = 1
STATE_COUNTDOWN = 2
STATE_EXPLODED  = 3

# Variables de control
current_state   = STATE_CONFIG
time_left       = 20   # Valor inicial (ajustable)
deact_step      = 0    # Paso actual de la secuencia de desactivación

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
        deact_step = 0  # reiniciar secuencia de desactivación

    elif current_state == STATE_COUNTDOWN:
        # 1) Chequear secuencia de desactivación: A → B → A → shake
        if deact_step == 0 and button_a.was_pressed():
            deact_step = 1
        elif deact_step == 1 and button_b.was_pressed():
            deact_step = 2
        elif deact_step == 2 and button_a.was_pressed():
            deact_step = 3
        elif deact_step == 3 and accelerometer.was_gesture("shake"):
            # Secuencia completa: volver a configuración
            time_left = 20
            current_state = STATE_CONFIG
            deact_step = 0
            continue
        # Si se presionó A/B o shake fuera de orden, resetear
        elif (button_a.was_pressed() or button_b.was_pressed() or accelerometer.was_gesture("shake")):
            deact_step = 0

        # 2) Reinicio rápido con touch
        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            deact_step = 0
            continue

        # 3) Cuenta regresiva
        display.scroll(str(time_left))
        time_left -= 1
        if time_left <= 0:
            current_state = STATE_EXPLODED
            deact_step = 0

    elif current_state == STATE_EXPLODED:
        # Explosión
        display.show(Image.SKULL)
        music.play(music.WAWAWAWAA)
        # Reiniciar tras toque
        if pin_logo.is_touched():
            time_left     = 20
            current_state = STATE_CONFIG
            deact_step    = 0
```
Explicación de la secuencia de desactivación

Para permitir desactivar la bomba una vez armada, añadí una variable deact_step que funciona como índice dentro de la serie de pasos esperados:

Pulsar botón A

Pulsar botón B

Pulsar botón A

Agitar (shake)

Dentro del estado STATE_COUNTDOWN, antes de realizar el scroll y decrementar el tiempo, verifico en cada iteración si el usuario ha realizado la acción correcta para el paso actual. Si coincide, incremento deact_step. Cuando deact_step llega a 3 y se detecta el gesto de shake, entiendo que la secuencia completa fue ingresada correctamente: asigno el estado de nuevo a STATE_CONFIG, restauro el tiempo a 20 s y reseteo deact_step a 0.

Si el usuario presiona A, B o agita el dispositivo en un momento que no corresponde al paso esperado, se asume secuencia incorrecta y se reinicia deact_step a 0, de modo que cualquier intento equivocado obliga a empezar la secuencia desde el principio. Este enfoque simple de “máquina de estados” para la secuencia garantiza que solo la cadena exacta A–B–A–shake desactive la bomba, mientras que cualquier otro input la deja en cuenta regresiva.








