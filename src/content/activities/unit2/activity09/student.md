```py
from microbit import *

# Estados usando posiciones Y de LEDs:
# Y=0 (rojo), Y=2 (amarillo), Y=4 (verde)
estado = 0  # Empieza en rojo
ultimo_tiempo = running_time()

while True:
    ahora = running_time()
    
    # Estado ROJO (LED superior)
    if estado == 0:
        display.clear()
        display.set_pixel(2, 0, 9)  # LED central superior
        if ahora - ultimo_tiempo > 5000:  # 5 segundos
            estado = 1
            ultimo_tiempo = ahora
            
    # Estado VERDE (LED inferior)
    elif estado == 1:
        display.clear()
        display.set_pixel(2, 4, 9)  # LED central inferior
        if ahora - ultimo_tiempo > 5000:  # 5 segundos
            estado = 2
            ultimo_tiempo = ahora
            
    # Estado AMARILLO (LED central)
    elif estado == 2:
        display.clear()
        display.set_pixel(2, 2, 9)  # LED centro exacto
        if ahora - ultimo_tiempo > 2000:  # 2 segundos
            estado = 0
            ultimo_tiempo = ahora
```
### Funcionamiento:
1. 3 **estados básicos** (números 0, 1, 2)
2. 1 LED encendido por estado:
  * Rojo: LED central de la fila superior (Y=0)
  * Verde: LED central de la fila inferior (Y=4)
  * Amarillo: LED del centro absoluto (Y=2)
3. Transiciones automáticas cada 5-5-2 segundos
