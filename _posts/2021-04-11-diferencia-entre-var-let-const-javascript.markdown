---
layout: post
title:  "var vs let vs const en JavaScript"
date:   2021-04-11 02:00:00 -0500
author: Germán Escobar
image: /assets/images/birds.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

En JavaScript existen 3 formas de declarar una variable: `var`, `let` y `const`. En este post veremos cuáles son sus diferencias y cuándo usar cada una.<!-- more -->

El resumen sería el siguiente:

* `const`: se utiliza para declarar una constante que no se puede reasignar.
* `let`: se utiliza para declarar una variable que puede ser reasignada.
* `var`: no se recomienda utilizarla, a menos de que tengamos que ejecutar el código en un ambiente que no soporte ES6 (la versión JavaScript que se lanzó en 2015[^1]).

Veamos ahora las razones por las cuáles no se recomienda usar `var`.

## Los problemas con var

JavaScript fue diseñado en **10 días** en 1994 para hacer scripts simples en Netscape (el navegador más popular de ese momento que hoy conocemos como Firefox). Nunca se creyó que se convertiría en el lenguaje más popular 20 años después.

Por el afán de lanzar el lenguaje rápidamente se cometieron muchos errores de diseño. Uno de ellos fue la forma en que se declaran las variables.

Por un lado, es posible declarar una  variable sin utilizar `var`, `let` o `const`, lo que crea una variable global en la aplicación[^2]:

```javascript
// mala práctica, crea una variable global
nombre = "Pedro"
```

Sin embargo, esto se puede evitar utilizando la declaración `'use strict'` al principio del archivo:

```javascript
'use strict'
nombre = "Pedro" // error!
```

El problema de las variables declaradas con `var` es que se comportan diferente que en la mayoría de lenguajes de programación, que limitan el alcance **por bloque**, es decir, entre llaves (`{` y `}`). Esto no ocurre en JavaScript cuando utilizamos `var`. Por ejemplo:

```javascript
if (true) {
  var color = "negro"
}
console.log(color) // "negro"
```

Con `var`, la única forma de limitar el alcance es dentro de una función:

```javascript
function miFuncion() {
  var color = "negro"
}
console.log(color) // error
```

## let y const

En ES6 (2015) se introdujeron dos nuevas formas de declarar una variable (además de muchas otras mejoras al lenguaje): `let` y `const`. A diferencia de `var`, `let` y `const` utilizan alcance por bloque:

```javascript
if (true) {
  const color = "negro"
}
console.log(color) // error!
```

El alcance por bloque se considera una mejor practica porque limita aún más el alcance de las variables, evitando colisiones con los nombres de otras variables y funciones.

La diferencia entre `let` y `const`  es que, por un lado, una variable declarada con `const` no se puede reasignar:

```javascript
const color = "negro"
color = "verde" // error!
```

Mientras que con `let` si:

```javascript
let color = "negro"
color = "verde" // bien
```

Por otro lado, una variable `let` se puede declarar sin un valor inicial, `const` necesita obligatoriamente un valor inicial:

```javascript
let var1; // undefined  
const var2; // error!
```

## Hoisting

Otra diferencia de `var` en relación con `let` y `const` es la forma en que funciona el **hoisting**.

**Hoisting** es un mecanismo por el cuál las declaraciones de variables y funciones se mueven al inicio de su alcance antes de que se ejecute el código. Por ejemplo, el siguiente codigo:

```javascript
console.log(saludo)
var saludo = "hola!"
```

se interpretaría de la siguiente forma cuando se ejecuta:

```javascript
var saludo;
console.log(saludo) // undefined
saludo = "hola"
```

Fíjate que la variable `saludo` se movió al principio del archivo y se inicializó con `undefined`.

`let` y `const` funcionan de forma similar, pero no se inicializan con `undefined`. Por lo tanto, si modificamos el código anterior utilizando `let` o `const` nos saldría un error:

```javascript
console.log(saludo) // error!
let saludo = "hola!" // utilizando let
```

## Conclusión

La diferencia entre estas tres formas de declarar variables es quizá una de las preguntas más frecuentes entre las personas que están empezando con JavaScript.

Si el lenguaje hubiese estado bien diseñado desde el principio nos hubiésemos evitado toda esta confusión. Sin embargo, la adición de `let` y `const` era necesaria para escribir código más predecible y mantenible.  Por esta razón se considera mala práctica utilizar `var`, aunque todavía se ve en proyectos antiguos.

---

[^1]: Realmente es la versión de la especificación sobre la que se basa JavaScript, llamada ECMAScript.

[^2]: Una variable global es una variable que es visible desde cualquier parte del programa. Las variables globales se consideran una mala práctica porque pueden ocasionar colisiones de nombres con otras variables o funciones. Estos problemas son difíciles de encontrar.
