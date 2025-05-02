```js
let useSine = true; // Variable para alternar entre seno y coseno

function setup() {
  createCanvas(800, 800);
  noLoop(); // Para que el patrón se genere una sola vez
}

function draw() {
  background(255); // Fondo blanco
  let numCircles = 100; // Número de círculos a dibujar

  for (let i = 0; i < numCircles; i++) {
    // Generar parámetros aleatorios
    let angle = random(TWO_PI); // Ángulo aleatorio
    let radius = random(50, 200); // Radio aleatorio entre 50 y 200
    let x, y;

    // Usar seno o coseno para calcular las posiciones
    if (useSine) {
      x = width / 2 + cos(angle) * radius; // Posición X usando coseno
      y = height / 2 + sin(angle) * radius; // Posición Y usando seno
    } else {
      x = width / 2 + sin(angle) * radius; // Posición X usando seno
      y = height / 2 + cos(angle) * radius; // Posición Y usando coseno
    }

    let size = random(10, 50); // Tamaño aleatorio del círculo

    // Dibujar el círculo
    fill(random(255), random(255), random(255), 150); // Color aleatorio con transparencia
    noStroke();
    ellipse(x, y, size, size);
  }
}

// Cambiar entre seno y coseno al presionar una tecla
function keyPressed() {
  if (key === 's' || key === 'S') {
    useSine = true; // Usar seno
    redraw(); // Redibujar el patrón
  } else if (key === 'c' || key === 'C') {
    useSine = false; // Usar coseno
    redraw(); // Redibujar el patrón
  }
}

```
