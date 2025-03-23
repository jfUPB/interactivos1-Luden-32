```python
from microbit import *
import utime

# Lista de imágenes a mostrar
imagenes = [Image.HEART, Image.HAPPY, Image.SAD]

# Lista de textos a mostrar
textos = ["Hola", "Adiós"]

while True:
    # Mostrar cada imagen de la lista
    for img in imagenes:
        display.show(img)
        utime.sleep(1)    # Espera 1 segundo
        display.clear()
        utime.sleep(0.5)  # Pausa breve entre imágenes

    # Mostrar cada texto de la lista de forma deslizante
    for txt in textos:
        display.scroll(txt)
        utime.sleep(0.5)  # Pausa breve entre textos

```

---

### Explicación del Funcionamiento

- **Selección de imágenes y textos:**
    
    Se definen dos listas, una para las imágenes (por ejemplo, un corazón, una cara feliz y una cara triste) y otra para los textos ("Hola" y "Adiós").
    
- **Bucle principal:**
    
    Dentro del `while True`, el programa recorre cada lista:
    
    - Para las imágenes, se muestra cada una durante 1 segundo y luego se borra la pantalla brevemente.
    - Para los textos, se utiliza `display.scroll()` para desplazarlos a lo largo de la pantalla LED.
- **Pausa y actualización:**
    
    Se utilizan pausas (`utime.sleep`) para dar tiempo a que el usuario pueda ver cada imagen o texto antes de pasar al siguiente, garantizando una transición visual clara.
