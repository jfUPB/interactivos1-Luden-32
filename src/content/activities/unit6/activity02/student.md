### **Pausa Activa 1: ¿Cómo accedo a Internet?**

En mi casa me conecto a través de Wi-Fi, que viene de un módem enlazado por cable al proveedor de Internet. En la universidad, suelo usar también Wi-Fi, aunque si necesito una conexión más estable (por ejemplo, para enviar archivos pesados o trabajar en red), me conecto por cable Ethernet. Si por alguna razón se cae la conexión (como un corte del Wi-Fi o se desconecta el cable), me quedo sin forma de salir a Internet. Es como si mi computadora estuviera lista para salir, pero la puerta del garaje estuviera cerrada: no puede ir a ningún sitio.

---

### **Pausa Activa 2: Cliente y servidor en la vida real**

Un ejemplo cotidiano: ir a una cafetería.

- **Cliente**: Yo, cuando hago mi pedido.
- **Servidor**: El mesero o barista.
- **Solicitud**: “Un café con leche, por favor.”
- **Respuesta**: Me trae el café con leche preparado.
    
    Así funciona también la web: un navegador pide algo y un servidor responde con lo que corresponde.
    

### **Pausa Activa 3: Entendiendo una URL**

Tomemos como referencia YouTube, así que uso este enlace:

https://www.youtube.com/watch?v=knVRvF2t7uc

- **Protocolo**: `https://` (comunicación segura)
- **Dominio**: `www.youtube.com`
- **Ruta**: `/watch` (ver contenido)
- **Parámetro**: `knVRvF2t7uc` (le dice al servidor qué video cargar)

Si solo escribo el dominio, me lleva a la página principal, porque el servidor interpreta que no le estoy pidiendo algo específico.

### **Pausa Activa 4: HTTP vs. Protocolos seriales**

Ambos permiten enviar datos, pero son mundos diferentes:

- **Similitud**: Tienen reglas para enviar y recibir mensajes correctamente.
- **Diferencia**: HTTP maneja solicitudes más complejas y variadas, mientras que los protocolos seriales son muy básicos, ideales para comunicar hardware directamente.
    
    HTTP necesita más estructura porque trabaja entre dispositivos remotos, manejando imágenes, formularios, seguridad, etc. Es como una autopista gigante frente a una callecita barrial.
    

### **Pausa Activa 5: ¿Qué hace cada lenguaje en una página de login?**

En una página típica de inicio de sesión:

- **HTML**: Construye los campos para escribir usuario y contraseña, y coloca los botones y textos.
- **CSS**: Se encarga de que todo se vea bien: colores, tipografías, márgenes.
- **JavaScript**: Valida en tiempo real si llenaste los campos, te avisa si algo está mal, e incluso puede enviarte a otra página si entraste correctamente.

### **Pausa Activa 7: ¿Bucle draw() o eventos?**

El bucle `draw()` en p5.js redibuja todo cada segundo muchas veces, aunque no haya cambios. En cambio, los **eventos** se activan solo cuando pasa algo (como presionar una tecla o hacer clic).

- **Modelo por eventos**: Más eficiente, especialmente en interfaces gráficas o móviles, donde no se necesita redibujar constantemente.
- ¿Sería útil tener `draw()` siempre? No realmente, solo si estás haciendo animaciones o videojuegos donde algo cambia todo el tiempo.

### ⚙️ **Pausa Activa 8: JavaScript en cliente y servidor**

Usar el mismo lenguaje en ambos lados tiene ventajas enormes:

- Aprendes una sola sintaxis.
- Puedes reutilizar funciones (por ejemplo, validaciones).
- El desarrollo es más rápido y organizado.
    
    Esto es una de las razones por las que JavaScript ha ganado tanto terreno.
    

### **Pausa Activa Final: HTTP vs WebSockets / Socket.IO**

- **HTTP**: Ideal para cargar páginas, enviar formularios, o hacer una consulta ocasional. Cada acción es una nueva conversación.
- **WebSockets / Socket.IO**: Conexión constante, como si se abriera una línea telefónica entre cliente y servidor. Ideal para apps en tiempo real.

**Ejemplos de uso de WebSockets:**

- Juegos online multijugador.
- Apps de mensajería en vivo como WhatsApp Web.
- Herramientas colaborativas tipo Google Docs o Figma.
- Apps de delivery que muestran el progreso de un pedido.
