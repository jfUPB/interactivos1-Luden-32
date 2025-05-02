## Enlaces

**Generative Design:** 

http://www.generative-gestaltung.de/2/sketches/?01_P/P_3_1_2_02

**p5.js:**

https://p5js.org/examples/classes-and-objects-connected-particles/

En el diseño del primer sitio, Generativa Design, me gustó un programa en especial. Aunque cabe recalcar que no tiene nada de especial como tal, era debido a que se podía, según las teclas presionadas en mi teclado, poder crear lo que parecía una ruta de trasporte, con esquinas, curvas, anormalidades, puntos especiales y mas.

En cuanto al segundo sitio, lo que me gustó fue el que al mover el cursor rapido, los círculos conectados conservaban parte de esa velocidad (o eso parecía) y entre mas rapido lo movía, mas largo era el segmento entre círculos.

### analisis de p5.js

Este código es un sketch de **p5.js** que crea un efecto visual donde al arrastrar el mouse se generan partículas conectadas por líneas que se desvanecen con el tiempo. Aquí está la explicación paso a paso:

---

### **Estructura General**

1. **Paths (Trayectorias)**: Son colecciones de partículas que forman un trazo continuo.
2. **Partículas**: Círculos de colores que se mueven y se conectan con líneas blancas.
3. **Fade-out**: Tanto las partículas como las líneas se desvanecen gradualmente.

---

### **Variables Clave**

- **`paths`**: Almacena todas las trayectorias activas.
- **`framesBetweenParticles`**: Controla la frecuencia de creación de partículas (cada 5 frames).
- **`particleFadeFrames`**: Tiempo (en frames) que tarda una partícula en desvanecerse (300 frames).
- **`previousParticlePosition`**: Guarda la posición anterior del mouse para calcular la velocidad de las partículas.

---

### **Funciones Principales**

1. **`setup()`**:
    - Configura el lienzo y el modo de color **HSB** (tono, saturación, brillo).
    - Inicializa **`previousParticlePosition`**.
2. **`draw()`**:
    - Actualiza y dibuja todas las trayectorias en cada frame.
3. **Eventos del Mouse**:
    - **`mousePressed()`**: Inicia una nueva trayectoria.
    - **`mouseDragged()`**: Añade partículas al arrastrar, controlando la frecuencia con **`framesBetweenParticles`**.
    - **`createParticle()`**: Crea partículas con velocidad basada en el movimiento del mouse.

---

### **Clases**

### **`Path` (Trayectoria)**

- **Propiedades**:
    - **`particles`**: Lista de partículas en la trayectoria.
- **Métodos**:
    - **`addParticle()`**: Añade partículas con un tono (hue) que varía secuencialmente.
    - **`connectParticles()`**: Dibuja líneas entre partículas consecutivas con opacidad decreciente.
    - **`display()`**: Dibuja las partículas y líneas, eliminando las que ya están desvanecidas (usa un bucle hacia atrás para evitar errores al eliminar elementos del array).

### **`Particle` (Partícula)**

- **Propiedades**:
    - **`position`**, **`velocity`**: Posición y velocidad de la partícula.
    - **`hue`**: Color en formato HSB.
    - **`drag`**: Fricción que reduce la velocidad cada frame.
    - **`framesRemaining`**: Frames restantes antes de desvanecerse.
- **Métodos**:
    - **`update()`**: Actualiza posición, aplica fricción y reduce **`framesRemaining`**.
    - **`display()`**: Dibuja un círculo con opacidad decreciente.

---

### **Comportamiento Visual**

1. **Al arrastrar el mouse**:
    - Se crean partículas en intervalos regulares.
    - Cada partícula tiene un color único que cambia en un ciclo (que se incrementa en 30° por partícula).
    - Las partículas se mueven con una velocidad inicial basada en la dirección del arrastre.
2. **Efecto de Desvanecimiento**:
    - Las partículas y líneas pierden opacidad proporcional a **`framesRemaining`**.
    - Cuando **`framesRemaining`** llega a 0, las partículas se eliminan.
3. **Líneas Conectivas**:
    - Las líneas blancas conectan partículas consecutivas en la misma trayectoria.
    - Su opacidad depende de la partícula más antigua de la pareja.

### Análisis de Generative Design

### **Características Principales**

1. **Texto Convertido en Arte Visual:**
    - Cada carácter del texto (**`textTyped`**) se dibuja con formas geométricas, imágenes SVG o iconos en lugar de letras tradicionales.
    - Ejemplo: Los espacios, comas, puntos, etc., tienen diseños personalizados con rotaciones y traslaciones.
2. **Interactividad:**
    - **Arrastrar:** Mueve la vista manteniendo clic izquierdo.
    - **Zoom:** Usa las flechas arriba/abajo para hacer zoom in/out.
    - **Editar Texto:** Borrar con **`BACKSPACE`**, añadir saltos de línea con **`ENTER`**.
    - **Guardar Imagen:** **`CTRL`** para guardar como PNG.
    - **Nuevo Diseño:** **`ALT`** cambia la semilla aleatoria para variaciones.
3. **Elementos Gráficos:**
    - **Paleta de Colores:** 7 colores definidos en **`palette`** que cambian en cada nueva línea.
    - **SVG Personalizados:** Imágenes cargadas para espacios, signos de puntuación e iconos (ej: **`icon1`**, **`shapeComma`**).
    - **Efectos de Transformación:** Rotaciones y traslaciones dinámicas basadas en el carácter.

---

### **Estructura del Código**

### **Variables Globales**

- **textTyped:** Texto a visualizar.
- **Imágenes/SVG:** Carga de gráficos para espacios, signos y iconos.
- **Posición y Zoom:** Variables para controlar la vista (**`centerX`**, **`centerY`**, **`zoom`**).
- **Paleta de Colores:** Array **`palette`** con colores RGB.

### **Funciones Clave**

- **`preload()`:** Carga fuentes e imágenes antes de iniciar.
- **`setup()`:** Configura el canvas, fuentes, y variables iniciales.
- **`draw()`:**
    - Actualiza la vista con interacciones del mouse.
    - Dibuja cada carácter aplicando reglas visuales (**`switch(letter)`**).
    - Usa **`push()/pop()`** para aislar transformaciones (rotaciones, traslaciones).
- **Eventos (`mousePressed`, `keyReleased`, etc.):** Gestionan interacciones del usuario.
