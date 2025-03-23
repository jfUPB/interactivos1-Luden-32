### Experimento 1: Lectura del botón A

**¿Qué querías comprobar?**

Quería verificar que el micro:bit pueda detectar correctamente cuando se presiona el botón A, mostrando una respuesta visual en la pantalla (por ejemplo, cambiar la imagen mostrada).

**¿Cuál fue tu hipótesis?**

Hipotetizaba que al presionar el botón A, el micro:bit detectaría el cambio en su estado y, consecuentemente, cambiaría la imagen que muestra en su pantalla. Cuando el botón no esté presionado, se mostraría otra imagen.

**Código involucrado en el experimento:**

```python
from microbit import *

while True:
    if button_a.is_pressed():
        display.show(Image.HAPPY)
    else:
        display.show(Image.SAD)

```

**Descripción de los resultados:**

- Al iniciar el programa, el micro:bit muestra una carita triste.
- Al presionar el botón A, la imagen cambia a una carita feliz.
- Al soltar el botón, la imagen vuelve a la carita triste.

**Análisis de esos resultados:**

Los resultados muestran que el micro:bit está detectando correctamente el estado del botón A. Cuando se presiona, la función `button_a.is_pressed()` retorna `True` y el código cambia la imagen en la pantalla, confirmando el comportamiento esperado. La transición inmediata entre imágenes al presionar y soltar el botón demuestra una respuesta rápida del sistema a la entrada digital.

**Conclusiones:**

El experimento confirma que la lectura del botón A funciona como se esperaba. La hipótesis es correcta: la entrada digital del botón A se detecta de forma precisa, lo que permite ejecutar acciones (como cambiar la imagen en la pantalla) en respuesta a su estado.

---

### Experimento 2: Lectura del botón B

**¿Qué querías comprobar?**

Quería comprobar que el micro:bit también pueda detectar la acción sobre el botón B, realizando una acción distinta a la del botón A.

**¿Cuál fue tu hipótesis?**

Supuse que al presionar el botón B, el micro:bit ejecutaría una acción definida (por ejemplo, mostrando otra imagen) y, al soltarlo, se revertiría a un estado inicial o mostraría otra imagen distinta.

**Código involucrado en el experimento:**

```python
from microbit import *

while True:
    if button_b.is_pressed():
        display.show(Image.HEART)
    else:
        display.show(Image.ANGRY)

```

**Descripción de los resultados:**

- Inicialmente se muestra una imagen de enojo.
- Al presionar el botón B, la imagen cambia a un corazón.
- Al dejar de presionar, vuelve a mostrarse la imagen de enojo.

**Análisis de esos resultados:**

La lectura del botón B funciona de forma similar a la del botón A, confirmando que cada botón puede ser manejado de manera independiente. El cambio de imagen refleja correctamente la detección del estado del botón B, lo que valida la hipótesis.

**Conclusiones:**

El experimento demuestra que la entrada digital del botón B se lee correctamente y se puede utilizar para desencadenar acciones específicas en el micro:bit, tal como se esperaba.

---

### Experimento 3: Lectura del botón virtual del logo

**¿Qué querías comprobar?**

El objetivo era verificar que el micro:bit detecte la interacción con el botón virtual del logo, similar a los botones físicos A y B.

**¿Cuál fue tu hipótesis?**

Creí que al tocar el logo, el micro:bit identificaría el cambio de estado y activaría una respuesta programada (por ejemplo, mostrar una imagen determinada), demostrando que la entrada táctil es funcional.

**Código involucrado en el experimento:**

```python
from microbit import *

while True:
    if pin_logo.is_touched():
        display.show(Image.YES)
    else:
        display.show(Image.NO)

```

*Nota: Dependiendo de la versión del micro:bit y la documentación, la detección del toque en el logo puede implementarse a través de un pin táctil o un método específico.*

**Descripción de los resultados:**

- En reposo, el micro:bit muestra una imagen de “NO”.
- Al tocar el logo, la imagen cambia a “YES”.
- Al dejar de tocar, vuelve a la imagen inicial.

**Análisis de esos resultados:**

Los resultados indican que el micro:bit responde correctamente a la interacción táctil del logo, similar a como lo hacen los botones físicos. Esto confirma que la función utilizada para detectar el toque es confiable y se comporta de forma coherente con las expectativas.

**Conclusiones:**

El experimento valida que el botón virtual del logo es una entrada digital funcional en el micro:bit. La hipótesis se cumple, ya que el sistema reacciona al toque mostrando la imagen correspondiente, lo que permite utilizar esta entrada para futuras aplicaciones interactivas.
