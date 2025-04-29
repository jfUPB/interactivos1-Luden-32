**1. Pruebas de Configuración Inicial**

| **ID** | **Descripción** | **Estado Inicial** | **Acción** | **Fuente** | **Resultado Esperado (p5.js)** | **Resultado Esperado (micro:bit)** |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | Aumentar tiempo máximo | CONFIG | Presionar tecla A 50 veces | p5.js | Tiempo llega a 60 y no aumenta | Muestra "60" |
| T2 | Disminuir tiempo mínimo | CONFIG | Presionar tecla B 15 veces | micro:bit | Tiempo llega a 10 y no disminuye | Muestra "10" |
| T3 | Armar con tiempo inválido | CONFIG | Shake físico | micro:bit | No cambia de estado | Muestra icono "X" de error |

**2. Pruebas de Secuencia de Armas**

| **ID** | **Descripción** | **Estado Inicial** | **Acción** | **Fuente** | **Resultado Esperado (p5.js)** | **Resultado Esperado (micro:bit)** |
| --- | --- | --- | --- | --- | --- | --- |
| T4 | Armar desde UI | CONFIG | Click en botón "Armar" | p5.js | Cambia a ARMED por 1s | Muestra flecha Norte |
| T5 | Interrumpir armado | ARMADO | Touch en logo | micro:bit | Vuelve a CONFIG | Muestra icono de reinicio |

**3. Pruebas de Cuenta Regresiva**

| **ID** | **Descripción** | **Estado Inicial** | **Acción** | **Fuente** | **Resultado Esperado (p5.js)** | **Resultado Esperado (micro:bit)** |
| --- | --- | --- | --- | --- | --- | --- |
| T6 | Desactivación exitosa | COUNTDOWN | Secuencia A-B-A-S | Ambos | Reinicia a CONFIG | Muestra check verde |
| T7 | Desactivación fallida | COUNTDOWN | Secuencia A-B-B-S | micro:bit | Mantiene COUNTDOWN | Muestra icono "X" |
| T8 | Tiempo agotado | COUNTDOWN | Esperar 20s | Sistema | Explota y reproduce sonido | Muestra calavera y suena |

**4. Pruebas de Explosión**

| **ID** | **Descripción** | **Estado Inicial** | **Acción** | **Fuente** | **Resultado Esperado (p5.js)** | **Resultado Esperado (micro:bit)** |
| --- | --- | --- | --- | --- | --- | --- |
| T9 | Reinicio desde p5.js | EXPLODED | Click en pantalla | p5.js | Vuelve a CONFIG | Muestra cara feliz |
| T10 | Reinicio desde micro:bit | EXPLODED | Touch en logo | micro:bit | Vuelve a CONFIG | Sincroniza tiempo |

**5. Pruebas de Comunicación Cruzada**

| **ID** | **Descripción** | **Estado Inicial** | **Acción** | **Fuente** | **Resultado Esperado (p5.js)** | **Resultado Esperado (micro:bit)** |
| --- | --- | --- | --- | --- | --- | --- |
| T11 | Control mixto A | CONFIG | Aumentar tiempo desde micro | micro:bit | Tiempo aumenta en 1 | Muestra nuevo valor |
| T12 | Control mixto B | COUNTDOWN | Enviar comando T desde p5 | p5.js | Reinicio inmediato | Muestra cara feliz |
