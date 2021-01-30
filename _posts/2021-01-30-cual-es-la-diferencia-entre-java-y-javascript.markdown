---
layout: post
title:  "¿Cuál es la diferencia entre Java y JavaScript?"
date:   2021-01-30 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/java-vs-javascript.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

Java y JavaScript son dos lenguajes de programación completamente diferentes. En este post explicaremos cuál es la diferencia y por qué JavaScript incluye la palabra Java en su nombre.<!-- more -->

Java es un lenguaje de programación creado por [Sun Microsystems (adquirido por Oracle)](https://es.wikipedia.org/wiki/Sun_Microsystems){:target="\_blank"} a principios de los noventas y lanzado en 1995 con una propuesta muy revolucionaria: permitía ejecutar el mismo código en diferentes sistemas operativos[^1].

Por esa misma época, la compañía detrás de [Netscape](https://es.wikipedia.org/wiki/Netscape_Navigator){:target="\_blank"}, el navegador más popular en ese momento, necesitaba un lenguaje de programación simple para incluir en el navegador. Por eso contrataron a [Brendan Eich](https://es.wikipedia.org/wiki/Brendan_Eich){:target="\_blank"}, quien creó un lenguaje en 10 días al que llamaron **LiveScript**.

**LiveScript** estaba basado en otro lenguaje de programación llamado [Scheme](https://es.wikipedia.org/wiki/Scheme){:target="\_blank"}, nada que ver con Java. Pero Netscape decidió aprovechar la popularidad de Java: **renombraron LiveScript a JavaScript y alteraron su sintaxis para que se pareciera más a la de Java**.

Se puede decir que el nombre JavaScript no fue más que una movida de mercadeo de [Netscape](https://es.wikipedia.org/wiki/Netscape_Navigator){:target="\_blank"} que hoy genera mucha confusión y que obliga a los que enseñamos JavaScript a escribir posts que aclaren la situación 😉.

Paradójicamente, desde hace algunos años JavaScript superó a Java y se convirtió en el lenguaje más popular de la actualidad, así que Java puede que sea ahora el que se beneficie del nombre 🤷🏽‍♂️.

Desde el punto de vista técnico Java y JavaScript tienen las siguientes diferencias y similitudes:

* Java es un lenguaje compilado[^2], JavaScript es un lenguaje interpretado. Ver post [Lenguajes compilados e interpretados](/lenguajes-compilados-e-interpretados/) para más información.
* Los dos son **orientados a objetos**. Java utiliza **clases**, JavaScript utiliza **prototipos** (aunque ahora también soporta clases).
* Java es **estático** (se debe definir el tipo de cada variable explícitamente), JavaScript es **dinámico** (el tipo de las variables se infiere en tiempo de ejecución).
* Java es **fuertemente tipado** (no hace conversión de tipos implícitamente), JavaScript es **débilmente tipado** (hace conversión de tipos implícitamente)
* JavaScript es el único lenguaje que se puede ejecutar en todos los navegadores. Sin embargo, ahora también podemos ejecutarlo por fuera del navegador (como Java) utilizando [Node.js](https://nodejs.org/){:target="\_blank"}

Pero la principal enseñanza de este post es la siguiente: **nunca decir Java cuando nos refiramos a JavaScript**. Es más demorado decir JavaScript completo, pero nos exponemos menos. Muchos desarrolladores sufren un microinfarto cuando escuchan a alguien decir Java refiriéndose a JavaScript.

---

[^1]: En otros lenguajes como C y C++ era necesario adaptar y compilar el código para cada sistema operativo. Java introdujo la idea de la máquina virtual, que se encarga de solucionar las diferencias entre los sistemas operativos.

[^2]: Realmente Java es un lenguaje compilado e interpretado a la vez, pero esto lo explicamos mejor en el post referenciado.
