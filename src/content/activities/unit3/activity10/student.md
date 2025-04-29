### **Análisis sobre la técnica de máquina de estados**

**¿Por qué esta técnica es poderosa para la escalabilidad de tu aplicación en términos de concurrencia y manejo de eventos?**

La técnica de máquina de estados permite estructurar una aplicación como una serie de estados claramente definidos, en los cuales solo se permite un conjunto limitado de eventos válidos. Esto tiene varias ventajas:

- **Escalabilidad:**
    - Se pueden agregar nuevos estados o transiciones sin necesidad de reestructurar completamente el código.
- **Legibilidad y organización:**
    - El comportamiento de la aplicación se entiende mejor, ya que cada estado tiene reglas claras.
- **Manejo de eventos concurrentes:**
    - Al tener estados bien definidos, es más fácil decidir cómo reaccionar ante eventos que llegan al mismo tiempo (por ejemplo, datos desde el micro:bit y el teclado).
- **Robustez:**
    - Reduce el riesgo de errores, ya que los eventos mal ubicados simplemente no tienen efecto o son ignorados.

---

### Ventajas y desventajas del tipo de pruebas realizadas

**Ventajas:**

1. **Cobertura completa:**
    - Se testearon todas las combinaciones posibles de estados y eventos, incluyendo los no modelados.
2. **Detección de errores en tiempo de desarrollo:**
    - Se identifican problemas antes de que el usuario los experimente.
3. **Pruebas de integración:**
    - Se verificó la comunicación entre p5.js y micro:bit.

**Desventajas:**

- **Manuales:**
    - Las pruebas se realizaron manualmente, lo que puede ser lento y propenso a errores humanos.
- **No automatizadas:**
    - Al no tener pruebas automáticas, se requiere rehacer cada prueba con cada cambio.

---

### ¿Por qué son importantes las pruebas de regresión?

Las pruebas de regresión aseguran que los cambios en el código no rompen funcionalidades anteriores. Son especialmente importantes cuando:

- Se agregan nuevos estados o eventos.
- Se modifica la lógica de transición.
- Se integra con nuevas plataformas (como el micro:bit).

**No hacer pruebas de regresión puede provocar:**

- Fallos inesperados.
- Errores difíciles de rastrear.
- Funcionalidades que dejan de operar sin que se note de inmediato.
