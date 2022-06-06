---
layout: post
title:  "React useEffect"
date:   2022-06-06 02:00:00 -0500
author: Cristian Moreno
image: /assets/images/bg-images/react-use-effect.jpg
gravatar: /assets/images/people/cristian-moreno.jpg
---

`useEffect` es probablemente el [hook](https://guias.makeitreal.camp/react/react-hooks) más confuso e incomprendido en React. Hoy quiero aclararte eso.

Usamos hooks todo el tiempo en [Make It Real](http://makeitreal.camp/?utm_source=blog&utm_medium=web&utm_campaign=inbound&utm_content=react-use-effect) y comprender `useEffect` es crucial si vamos a escribir código React de estilo moderno.<!-- more -->

A continuación veremos:

- Qué es `useEffect`.
- Cómo ejecutar un effect(efecto) en cada `render`.
- Cómo ejecutar un efecto solo en el primer `render`.
- Cómo ejecutar un efecto en el primer `render` y volver a ejecutarlo cuando cambia una "dependencia".
- Cómo ejecutar un efecto con limpieza.

## ¿Qué es useEffect?

El hook [`useEffect`](https://is-tracking-link-api-prod.appspot.com/api/v1/click/5979720297742336/4956748072878080) nos permite realizar *side effects* (efectos secundarios) en nuestros componentes de función. Los side effects son acciones externas al código que se está ejecutando, por ejemplo:

- Llamadas a API
- Actualizar el DOM
- Suscribirse a eventos (event listeners).

Todos estos son efectos secundarios que podríamos necesitar que un componente realice en diferentes momentos.

## Ejecutar useEffect en cada render

El hook `useEffect` no devuelve ningún valor, sino que toma dos argumentos, siendo el primero obligatorio y el segundo opcional. El primer argumento **es la función callback del efecto que queremos que ejecute hook (es decir, el efecto en sí)**. Supongamos que queríamos colocar un mensaje `console.log()` dentro del callback del useEffect.

```javascript
import { useEffect } from "react";

export const FunctionComponent = () => {
  useEffect(() => {
    console.log("run for every componentrender");
  });

  return (
    // ...
  );
}
```

De forma predeterminada, el efecto establecido en el hook `useEffect` se ejecuta cuando el componente **se renderiza por primera vez** y **después de cada actualización**. Si ejecutamos el código anterior, notaremos que se genera el mensaje `console.log('run for every componentrender')` a medida que se renderiza nuestro componente. *Si* nuestro componente alguna vez se volviera a renderizar (por ejemplo, de un cambio de estado con algo como `useState`), el efecto se ejecutaría nuevamente.

A veces, volver a ejecutar un efecto en cada renderizado es exactamente lo que quieres. Pero la mayoría de las veces, **solo desea ejecutar el efecto en ciertas situaciones**, como en el primer renderizado.

## Cómo ejecutar el useEffect solo en el primer render

El segundo argumento del hook `useEffect` es opcional y es una **lista de dependencias** que nos permite decirle a React que *omita* la aplicación del efecto solo hasta que se den ciertas condiciones. En otras palabras, el segundo argumento del hook `useEffect` nos permite limitar **cuándo se ejecutará el efecto**. Si simplemente colocamos un array vacío como segundo argumento, así es como le decimos a React que solo ejecute el efecto en el renderizado inicial.

```javascript
import { useEffect } from "react";

export const FunctionComponent = () => {
  useEffect(() => {
    console.log("run only for first component render (i.e.component mount)");
  }, []);

  return (
    // ...
  );
}
```

Con el código anterior, el mensaje `console.log()` solo se activará cuando el componente se monte por primera vez y no se volverá a generar, incluso si el componente se vuelve a renderizar varias veces.

Esto es mucho más "eficiente" que ejecutar en cada renderizado, pero ¿no hay un término medio feliz? ¿Qué pasa si queremos rehacer el efecto, si algo cambia?

## Ejecutar el useEffect en el primer render y volver a ejecutarlo cuando cambie la dependencia

En lugar de hacer que un efecto se ejecute una vez al principio y en cada actualización, podemos intentar restringir el efecto para que se ejecute solo al principio y **cuando cambie cierta dependencia**.

Supongamos que queríamos lanzar un mensaje `console.log()` cada vez que cambiara el valor de una propiedad de estado. Podemos lograr esto colocando la propiedad de estado como una *dependencia* del callback del efecto. Mira el siguiente ejemplo de código:

```jsx
import { useState, useEffect } from "react";

export const FunctionComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(
      "run for first component render and re-run when 'count' changes"
    );
  }, [count]);

  return (
    <button onClick={()=> setCount(count + 1)}>
      Click to increment count and trigger effect
    </button>
  );
}
```

Arriba tenemos un botón en el template del componente responsable de cambiar el valor de la propiedad de estado de `count` cuando se hace click. Cada vez que se cambie la propiedad de estado `count` (es decir, cada vez que se haga click en el botón), notaremos que se ejecuta el callback del efecto y se activa el mensaje `console.log()`. 

## Ejecutar efecto con limpieza

Se ejecuta un callbak de efecto cada vez en el procesamiento inicial y cuando especificamos cuándo se debe ejecutar un efecto. El hook `useEffect` también brinda la capacidad de ejecutar una limpieza *después* del efecto. Esto se puede hacer especificando una función de retorne al final de nuestro efecto.

```jsx
import { useState, useEffect } from "react";

export const FunctionComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(
      "run for first component render and re-run when 'count' changes"
    );

    return () => {
      console.log("run before the next effectand when component unmounts");
    };
  }, [count]);

  return (
    <button onClick={()=> setCount(count + 1)}>
      Click to increment count and trigger effect
    </button>
  );
}
```

En el ejemplo anterior, notaremos que el mensaje de la función de limpieza se activa antes de que se ejecute el efecto deseado. Además, si nuestro componente alguna vez se desmonta, la función de limpieza también se ejecutará.

Un buen ejemplo de cuándo podríamos necesitar una limpieza es cuando configuramos una suscripción a nuestro efecto, pero queremos eliminar la suscripción cada vez que se realice la próxima llamada de suscripción, para evitar memory leaks.

A good example of when we might need a cleanup is when we set up a subscription in our effect but want to remove the subscription whenever the next subscription call is to be made, to avoid memory leaks.

Estas son principalmente todas las diferentes formas en que el hook `useEffect` se puede utilizar para ejecutar efectos secundarios en componentes. Te invito a ver esta [guia visual de useEffect](https://alexsidorenko.com/blog/useeffect/) de ALEX SIDORENKO que ilustra estos conceptos a través de una serie de GIF es a la vez inteligente y eficaz, especialmente para los estudiantes visuales. También hay una visualización de cómo funcionan las funciones de primera clase si quieres más.

[https://alexsidorenko.com/blog/useeffect/](https://alexsidorenko.com/blog/useeffect/)
