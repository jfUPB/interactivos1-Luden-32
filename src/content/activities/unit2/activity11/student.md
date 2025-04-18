# Funcionamiento
## Modo de configuración
* La bomba inciia en este modo sin realizar cuenta regresiva
* El tiempo inciial de al cuenta regresiva está fijado en 20 segundos
* Mediante el boton **UP** (A) y el botón **DOWN** (B) se puede ajustar el tiempo entre 10 - 60 segundos, incrementando o decrementando 1 segundo
* Se utiliza ultimate.ticks_ms() para gestionar la temporización sin bloquear la ejecución
## Modo armado
* Al detectar el gesto de shake (ARMED), se arma la bomba y comienza la cuenta regresiva.
* La pantalla de LED muestra el tiempo restante, actualizándose cada segundo.
* Si se presiona el botón TOUCH durante este modo, se detiene la cuenta regresiva y se vuelve al modo de configuración.
## Explosión
* Cuando la cuenta regresiva llega a 0, se activa la explosión.
* El speaker emite una alarma para simular la explosión de la bomba.
## Reinicio
* En cualquier momento, si se presiona el botón TOUCH, se reinicia el sistema y se retorna al modo de configuración, permitiendo reprogramar el tiempo de la bomba.
## Estados de la Máquina
### Configuración:
* Se muestra el tiempo programado (20 segundos por defecto).
* Se ajusta el tiempo con los botones UP y DOWN (rango de 10 a 60 segundos).
* Evento de transición: Shake (ARMED) → Pasa al modo ARMADO.
### ARMADO:
* Inicia la cuenta regresiva y se actualiza el display cada 1000 ms.
* Se decrementa el tiempo restante; cuando éste llega a 0, se produce la transición a EXPLOSIÓN.
* Evento de transición: Presionar TOUCH → Vuelve a CONFIGURACIÓN.
### EXPLOSIÓN:
* Al alcanzar 0 segundos, se activa la alarma en el speaker simulando la explosión.
* * Evento de transición: Presionar TOUCH → Reinicia la bomba y se retorna a CONFIGURACIÓN.
