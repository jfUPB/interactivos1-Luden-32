### Pausa activa – ¿Por qué es útil usar módulos o librerías en vez de escribir todo desde cero?  
Usar cosas como express, http, socket.io o path hace la vida mucho más fácil. Ahorran tiempo porque ya vienen con funciones listas, están bien probadas, tienen buena documentación y mucha gente las usa. En vez de complicarse reinventando la rueda, uno puede enfocarse en construir lo que realmente necesita.

### ¿Qué pasa si cambio la carpeta de archivos estáticos a una que no existe?  
Cuando cambié el express.static() a una carpeta que no existía (por ejemplo, 'archivos_cliente'), la página no cargó y el navegador soltó un error 404. También apareció un mensaje en la consola diciendo que no se encontró el recurso. Al volver a poner 'views', todo funcionó bien otra vez. Básicamente, si Express no encuentra la carpeta donde están los archivos, no puede servir nada.

### ¿Qué ocurre al cambiar la ruta del servidor?  
Si en el código del servidor cambiamos la ruta de /page1 a algo como /pagina_uno, al intentar abrir http://localhost:3000/page1 simplemente no carga. Pero si entramos a la nueva ruta que sí está definida (/pagina_uno), ahí sí responde bien. Es decir, la ruta que pongas en el servidor tiene que coincidir con la que usas en el navegador.

### Conexión y desconexión de clientes usando Socket.IO  
Cuando abrí page1, en la terminal salió algo como:
A user connected - ID: ...
Al abrir page2, apareció otro mensaje igual pero con otro ID.
Y cuando cerré cada pestaña, la terminal avisó con:
User disconnected - ID: ...
O sea que el servidor está pendiente de quién entra y sale en tiempo real.

### Intercambio de datos entre páginas  
En page1, se mostraba algo como:
Data: { x: 100, y: 150, width: 200, height: 300 }
Y en page2 llegaba un mensaje parecido, pero con otros valores. Eso muestra que ambas páginas están compartiendo datos y que el servidor los está enviando correctamente.

### Prueba clave – ¿Qué pasa si cambio de broadcast.emit a emit?  
Al cambiar socket.broadcast.emit() por socket.emit(), noté que el movimiento ya no se reflejaba en la otra página. Eso es porque emit solo manda el mensaje al mismo cliente que lo envía, mientras que broadcast.emit se lo manda a todos menos al que lo originó. Detalle clave.

### Cambio de puerto en el servidor  
Paré el servidor y cambié el puerto de 3000 a 3001. Cuando lo volví a prender, en la consola salió:
Server is listening on http://localhost:3001
Intenté entrar por el puerto viejo (3000) y no funcionó, como era de esperarse. Pero en 3001, todo andaba bien. Esto me dejó claro que el puerto es básicamente la “puerta” por donde se accede al servidor, y si la cambiás, tenés que entrar por la nueva.

