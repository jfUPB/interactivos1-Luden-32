# Semáforos Concurrentes en micro:bit

```python
from microbit import *
import utime

class Semaforo:
    def __init__(self, rojo, amarillo, verde, posicion):
        self.tiempos = {'rojo': rojo, 'amarillo': amarillo, 'verde': verde}
        self.estado = 'rojo'
        self.tiempo_restante = rojo
        self.pos = posicion             # columna en el display
        self.ultimo = utime.ticks_ms()  # marca de tiempo del último cambio
        self.prev_row = 0               # fila donde está el LED encendido
        display.set_pixel(self.pos, self.prev_row, 9)

    def update(self):
        ahora = utime.ticks_ms()
        # Cada 1000 ms decrementamos
        if utime.ticks_diff(ahora, self.ultimo) >= 1000:
            self.ultimo = ahora
            self.tiempo_restante -= 1

            if self.tiempo_restante <= 0:
                # Apagar el LED anterior
                display.set_pixel(self.pos, self.prev_row, 0)

                # Cambiar al siguiente estado
                if self.estado == 'rojo':
                    self.estado = 'verde'
                elif self.estado == 'verde':
                    self.estado = 'amarillo'
                else:
                    self.estado = 'rojo'

                # Cargar tiempo del nuevo estado
                self.tiempo_restante = self.tiempos[self.estado]

                # Decidir fila y brillo para representar color
                if self.estado == 'rojo':
                    row, bright = 0, 9    # rojo = fila 0, máximo brillo
                elif self.estado == 'amarillo':
                    row, bright = 2, 5    # amarillo = fila 2, brillo medio
                else:
                    row, bright = 4, 1    # verde = fila 4, brillo bajo

                self.prev_row = row
                display.set_pixel(self.pos, row, bright)

# Crear tres semáforos con sus tiempos y posiciones
semaforo1 = Semaforo(5, 2, 3, 0)  # 5s rojo, 2s amarillo, 3s verde, columna 0
semaforo2 = Semaforo(3, 1, 2, 2)  # 3s rojo, 1s amarillo, 2s verde, columna 2
semaforo3 = Semaforo(4, 3, 2, 4)  # 4s rojo, 3s amarillo, 2s verde, columna 4

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
```
Al diseñar los tres semáforos concurrentes mediante una clase Semáforo, aprovecho la reutilización y la modularidad: cada semáforo encapsula su propia lógica de tiempos, estado actual y método update(), lo que evita duplicar código y facilita cambios futuros (por ejemplo, ajustar tiempos o posición). La programación orientada a objetos me permite instanciar múltiples semáforos simplemente variando parámetros, manteniendo el while True principal libre de lógica interna de cada uno. Además, el patrón de máquina de estados asegura que la transición entre rojo, amarillo y verde sea coherente y se gestione de forma aislada en cada objeto, mejorando la legibilidad y el mantenimiento. En un proyecto real, esta estructura haría sencillo añadir más semáforos, nuevas fases (animaciones intermedias) o comportamientos adicionales (p. ej., modo de emergencia) sin tocar el loop global. Finalmente, la separación de la temporización (utime.ticks_ms()) y el refresco del display en el método update() garantiza un sistema reactivo y concurrente, crítico en aplicaciones embebidas donde múltiples procesos deben “correr” al mismo tiempo.
