### Estructura general y dependencias

- Es un sketch de p5.js (reconocible por las funciones **`setup()`**, **`draw()`**, etc.).
- Usa imágenes SVG almacenadas en la carpeta "data".
- Maneja interacciones de teclado y mouse para crear una herramienta de dibujo interactiva.

### Variables importantes

- **`c`**: Almacena el color actual.
- **`lineModuleSize`**: Tamaño del elemento de dibujo.
- **`angle`** y **`angleSpeed`**: Controlan la rotación y su velocidad.
- **`lineModule[]`**: Array para almacenar imágenes SVG.
- **`lineModuleIndex`**: Índice del elemento de dibujo actual.

### **Precarga de recursos (`preload()`)**

- Carga 4 imágenes SVG en las posiciones 1-4 del array **`lineModule`**.
- La posición 0 se usa para el modo línea básica.

### **Configuración inicial (`setup()`)**

- Crea un canvas de pantalla completa.
- Establece color inicial (dorado) y estilo de trazo.
- Oculta el cursor del mouse.

### **Lógica principal de dibujo (`draw()`)**

a. **Cuando se presiona el mouse:**

- Calcula posición (x,y) del mouse.
- Con SHIFT: Limita el movimiento a horizontal/vertical (usa **`clickPosX/Y`**).

b. **Transformaciones gráficas:**

- **`push()`**/**`pop()`** aíslan las transformaciones.
- **`translate()`** mueve el origen a la posición del mouse.
- **`rotate()`** gira según el ángulo acumulado.

c. **Dibujo del elemento:**

- Si hay imagen SVG: Aplica **`tint()`** con color actual y dibuja la imagen.
- Si es modo línea: Dibuja una línea diagonal con **`stroke()`**.
- Incrementa el ángulo para rotación continua.

### **Eventos de mouse (`mousePressed()`)**

- Inicia nuevo trazo con tamaño aleatorio (50-160px).
- Guarda posición inicial para el modo SHIFT.

### **Manejo de teclado**

a. **Teclas constantes (`keyPressed()`):**

- Flechas: Ajustan tamaño y velocidad de rotación.

b. **Al soltar teclas (`keyReleased()`):**

- **`s`**: Guarda imagen PNG.
- **`d`**: Invierte dirección y ángulo.
- **`1-4`**: Colores predefinidos.
- **`Espacio`**: Color aleatorio.
- **`5-9`**: Seleccionan elementos de dibujo.
- **`Delete/Bksp`**: Limpia pantalla.

### **Características especiales**

- **Rotación dinámica:** El ángulo se actualiza continuamente con **`angleSpeed`**.
- **Sistema de coordenadas:** Las transformaciones se aplican localmente para cada elemento.
- **Modo línea restringida:** SHIFT fuerza trazos rectos (horizontal/vertical).
- **Responsividad:** **`windowResized()`** ajusta el canvas al tamaño de ventana.

### **Flujo de trabajo típico:**

1. Usuario arrastra mouse para dibujar.
2. El elemento gira automáticamente mientras se mueve.
3. Teclas modifican propiedades en tiempo real:
    - Cambio de color/transparencia
    - Ajuste de tamaño/velocidad
    - Selección de diferentes "pinceles"
4. Sistema mantiene estado entre interacciones.

### **Detalles técnicos clave**

- Uso de **`tint()`** para colorear imágenes SVG.
- **`push()`**/**`pop()`** preservan el estado de transformación.
- El cálculo de posición con SHIFT usa diferencia absoluta entre ejes.
- La rotación se acumula (**`angle += angleSpeed`**) para efecto continuo.
- Las imágenes SVG se escalan dinámicamente con **`lineModuleSize`**.
