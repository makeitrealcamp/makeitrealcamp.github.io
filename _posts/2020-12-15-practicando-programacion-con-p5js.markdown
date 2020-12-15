---
layout: post
title:  "Practicando programación con p5.js"
date:   2020-12-15 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/practicando-programacion-p5js.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

[p5.js](https://p5js.org/){:target="\_blank"}  es una librería de código abierto escrita en JavaScript que nos permite crear dibujos y animaciones en el navegador utilizando código. En este post vas a aprender cómo empezar con esta interesante librería y algunas ideas para practicar.<!-- more -->

**Nota:** Para aprovechar más esta librería deberías conocer los conceptos básicos de programación en JavaScript como variables, condicionales, ciclos, arreglos, funciones y objetos literales. Si no es así te recomendamos [ver estos videos primero](https://www.youtube.com/playlist?list=PLxyfMWnjW2kvlPTL3CXFtb40587w62wMs){:target="\_blank"} y consultar [la sección de JavaScript en las guías de Make It Real](https://guias.makeitreal.camp/javascript-i){:target="\_blank"}.

Existe un editor de [p5.js](https://p5js.org/){:target="\_blank"} que puedes abrir desde tu navegador en [https://editor.p5js.org/](https://editor.p5js.org/){:target="\_blank"} y es la forma más fácil de empezar a crear con esta librería.

Cuando abres el editor de [p5.js](https://p5js.org/){:target="\_blank"} vas a encontrar código en el archivo `sketch.js`. Si oprimes el botón de ejecutar (el botón "play" rojo en la parte superior) vas a ver algo similar a lo siguiente:

<img src="/assets/images/p5js-editor.jpg" alt="Editor de p5.js" class="photo border">

En el código hay dos funciones: `setup` y `draw`. `setup` se invoca una única vez cuando oprimes ejecutar, mientras que `draw` se ejecuta dentro de un ciclo hasta que detienes el programa (con el botón de detener).

[p5.js](https://p5js.org/){:target="\_blank"} incluye funciones para hacer todo tipo de cosas interesantes: dibujar figuras, trabjar con imágenes y video, escuchar eventos y hacer animaciones, entre muchas otras.

La función `createCanvas` que está en `setup` crea el lienzo (canvas) sobre el que vamos a “dibujar”. Y en la función `draw` se invoca `background`, que cambia el color de fondo.

Vamos ahora a pintar un cuadrado en nuestro lienzo utilizando la función `square`. Modifica la función `draw` para que quede de la siguiente forma.

```javascript
function draw() {
  background(220);
  square(10, 10, 30);
}
```

Si oprimes ejecutar deberías ver un cuadrado de fondo blanco en el lienzo. La función `square` recibe 3 argumentos: los dos primeros son las coordenadas para ubicar el cuadrado y el tercero es la longitud de los lados.

La mayoría de funciones que nos permiten dibujar reciben primero la ubicación. [p5.js](https://p5js.org/){:target="\_blank"} utiliza un sistema de coordenadas que inicia en la esquina superior izquierda con la posición (0, 0), y termina en la esquina inferior con el ancho y el alto del lienzo, que es (400,400) por defecto.

## Creando animaciones

Como el método `draw` se invoca constantemente por [p5.js](https://p5js.org/){:target="\_blank"} podemos crear animaciones utilizando variables. Por ejemplo, en vez de que la coordenada `x` sea siempre 10, vamos a utilizar una variable que vamos a ir aumentando para crear la ilusión de movimiento horizontal:

```javascript
let x = 0;
function draw() {
  background(220);
  square(x, 10, 30);
  x++
}
```

Si ejecutas el código vas a ver al cuadrado moviéndose por la pantalla hasta que desaparece. Mejoremos el código para que se devuelva cuando llegue a los límites:

```javascript
let x = 0
let d = 1
function draw() {
  background(220);
  square(x, 10, 30);

  x += d
  if (x < 0 || x > 370) {
    d = d * -1
  }
}
```

En este código agregamos una variable `d` que va a ser 1 o -1, y que vamos a utilizar para cambiar la dirección de `x` (incrementa cuando es 1 y decrementa cuando es -1). Cuando llegamos a los límites invertimos `d`. El resultado es el siguiente:

<img src="/assets/images/p5js-animation.gif" alt="Animación de p5.js" class="photo border">

## Escuchando eventos

Con [p5.js](https://p5js.org/){:target="\_blank"} puedes escuchar eventos del teclado, del mouse y de aceleración (para móviles).

Creemos un nuevo ejemplo en donde podamos mover el cuadrado con las flechas del teclado:

```javascript
let x = 100
let y = 100
function draw() {
  background(220);

  if (keyIsDown(LEFT_ARROW)) x -= 2
  if (keyIsDown(RIGHT_ARROW)) x += 2
  if (keyIsDown(UP_ARROW)) y -= 2
  if (keyIsDown(DOWN_ARROW)) y += 2

  square(x, y, 30);
}
```

En este ejemplo estamos usando la función `keyIsDown` que nos permite saber si una tecla está siendo oprimida. Para conocer todos los eventos que puedes [consultar la documentación](https://p5js.org/reference/){:target="\_blank"}, en la sección de Eventos.

Puedes ver el resultado de este ejemplo en [este enlace](https://editor.p5js.org/germanescobar/sketches/C4GEq4_ou){:target="\_blank"}.

## Algunas ideas

¿Qué tal si empezamos a agregar obstáculos en nuestro ejemplo anterior o si creamos un laberinto? Las posibilidades de lo que puedes hacer en [p5.js](https://p5js.org/){:target="\_blank"} son infinitas.

Una función que puede ser muy útil para tus creaciones es `random`, que permite generar valores aleatorios dentro de un rango de números. Por ejemplo, podemos generar círculos de forma aleatoria por toda la pantalla:

```javascript
function draw() {
  for (let i=0; i < 100; i++) {
    const x = random(0, 400)
    const y = random(0, 400)
    circle(x, y, 20)
  }
}
```

Para más inspiración puedes ir al sitio de [p5.js](https://p5js.org/){:target="\_blank"} donde vas a encontrar una [galería de ejemplos](https://p5js.org/examples/){:target="\_blank"} y [creaciones de otras personas](https://showcase.p5js.org/#/){:target="\_blank"}. También hay un canal en YouTube llamado [The Coding Train](https://www.youtube.com/user/shiffman){:target="\_blank"} donde puedes aprender y seguir tutoriales de [p5.js](https://p5js.org/){:target="\_blank"}.

En una próxima entrada vamos a utilizar [p5.js](https://p5js.org/){:target="\_blank"} y otra librería llamada [ml5.js](https://ml5js.org/){:target="\_blank"} para hacer reconocimiento facial con Machine Learning en el navegador!
