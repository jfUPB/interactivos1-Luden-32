```python
from microbit import *
import music

while True:
    # Sonido 1: Tono único (440 Hz) durante 500 ms
    music.pitch(440, 500)
    sleep(500)  # Pausa entre sonidos

    # Sonido 2: Melodía corta con 4 notas (C4, D4, E4, F4)
    melody = ['C4:4', 'D4:4', 'E4:4', 'F4:4']
    music.play(melody, wait=True)
    sleep(1000)  # Pausa antes de repetir el ciclo

```

---

### Documentación y Descripción del Proceso

- **Código:**
    - Se importa el módulo `music` para utilizar funciones de generación de sonidos.
    - En el bucle principal, se utiliza `music.pitch()` para reproducir un tono fijo de 440 Hz durante 500 ms.
    - Luego, se define una breve melodía con cuatro notas y se reproduce usando `music.play()`.
    - Se añaden pausas (`sleep`) para separar los sonidos y evitar solapamientos.
- **Proceso:**
    
    Se investigó el módulo `music` disponible en MicroPython para el micro:bit, probando tanto la generación de tonos individuales como la reproducción de melodías. La combinación de ambas técnicas permite experimentar con diferentes tipos de salidas sonoras usando el buzzer o speaker integrado (en micro:bit v2 o mediante conexión externa en versiones anteriores).
