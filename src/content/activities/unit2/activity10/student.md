**Descripción del Problema**

El micro:bit muestra una serie de imágenes en pantalla siguiendo este orden:

- Cara Feliz → durante 1.5 segundos
- Cara Sonriente → durante 1 segundo
- Cara Triste → durante 2 segundos
- Se repite el ciclo

Si en cualquier instante el usuario presiona el botón A, se produce el siguiente cambio en la secuencia:

- De Feliz pasa a Triste
- De Sonriente pasa a Feliz
- De Triste pasa a Sonriente

**Concurrencia en el Programa**

El programa alcanza la concurrencia mediante los siguientes métodos:

- Se separa el control del tiempo de la detección de eventos, verificando de forma continua si ha transcurrido el tiempo asignado a cada estado sin interrumpir la ejecución.
- Se utiliza `utime.ticks_ms()` para medir el tiempo sin detener el flujo del programa con `sleep()`, lo cual permite capturar eventos de botones en cualquier momento.
- Se realiza un cambio inmediato del estado al detectar un evento de botón, interrumpiendo la secuencia normal según sea necesario.

**Análisis del Código**

El código define una máquina de estados compuesta por cuatro etapas:

- **STATE_INIT:** Inicializa el sistema y muestra la cara feliz.
- **STATE_HAPPY:** Exhibe la cara feliz y espera 1.5 segundos.
- **STATE_SMILE:** Muestra la cara sonriente y espera 1 segundo.
- **STATE_SAD:** Presenta la cara triste y espera 2 segundos.

En cada estado se contemplan eventos que conducen a otra transición:

- El transcurso del tiempo → provoca el cambio al siguiente estado.
- La presión del botón A → cambia a un estado alternativo.

**Vectores de Prueba**

Para verificar el correcto funcionamiento del programa se utilizaron los siguientes escenarios de prueba:

- **Vector de Prueba 1 - Secuencia Normal**
    - *Condición Inicial:* El programa inicia en STATE_HAPPY.
    - *Eventos Realizados:* No se presiona ningún botón y se permite el paso del tiempo.
    - *Resultado Esperado:*
        - STATE_HAPPY → STATE_SMILE (tras 1.5 s).
        - STATE_SMILE → STATE_SAD (tras 1 s).
        - STATE_SAD → STATE_HAPPY (tras 2 s).
    - *Resultado Obtenido:* La secuencia se ejecutó correctamente.
- **Vector de Prueba 2 - Interrupción en STATE_HAPPY**
    - *Condición Inicial:* El programa se encuentra en STATE_HAPPY.
    - *Eventos Realizados:* Se presiona el botón A antes de que se completen los 1.5 segundos.
    - *Resultado Esperado:*
        - El estado cambia de forma inmediata a STATE_SAD, sin esperar el tiempo completo.
        - Luego continúa la secuencia normal (STATE_SAD → STATE_HAPPY).
    - *Resultado Obtenido:* La interrupción se realizó de manera correcta.
- **Vector de Prueba 3 - Interrupción en STATE_SAD**
    - *Condición Inicial:* El programa se halla en STATE_SAD.
    - *Eventos Realizados:* Se presiona el botón A antes de que pasen los 2 segundos estipulados.
    - *Resultado Esperado:*
        - El programa cambia de inmediato a STATE_SMILE.
        - Posteriormente sigue la secuencia normal (STATE_SMILE → STATE_SAD).
    - *Resultado Obtenido:* La interrupción se ejecutó de forma correcta.
