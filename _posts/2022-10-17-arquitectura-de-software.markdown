---
layout: post
title:  "La arquitectura de software"
date:   2022-10-17 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/software-architecture.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

La arquitectura de software y el rol del arquitecto son de los conceptos más incomprendidos en el desarrollo de software. En este post vamos a explicar qué es la arquitectura, qué hace un arquitecto y cómo se relaciona con otros temas como patrones de diseño y los principios SOLID.<!-- more -->

La arquitectura de software surge en los años 70’s como una analogía a la construcción civil. En ese momento se creía que construir software era similar a construir un edificio o un puente, y se tomaron varios procesos y roles de esa disciplina, entre ellos la arquitectura y el arquitecto.

En la construcción civil, la arquitectura define el diseño de lo que se va a construir a través de planos y maquetas que otros después construyen.

La idea de la arquitectura de software (y el arquitecto) es que fuera similar a la construcción civil y para eso se crearon lenguajes de modelado como [UML (Unified Modeling Language)](https://es.wikipedia.org/wiki/Lenguaje_unificado_de_modelado){:target="\_blank"}, que le permitía a los arquitectos plasmar el diseño de una aplicación en diagramas que después otros construirían.

Sin embargo, lo que nos dimos cuenta en los años 90’s es que el software es muy diferente a la construcción civil. Aunque definimos nuevos procesos como el desarrollo ágil (y descartamos otros como el [modelo en cascada](https://es.wikipedia.org/wiki/Desarrollo_en_cascada){:target="\_blank"}). Es hora también de repensar la arquitectura y el rol del arquitecto.

## ¿Qué es la arquitectura de software?

La arquitectura de software, en términos generales, define la estructura y el diseño técnico de una aplicación: cuáles van a ser los componentes principales, cuál va a ser la responsabilidad de cada uno, y cómo se van a comunicar entre ellos. A la arquitectura se le conoce hoy como el **diseño del sistema**[^1].

Por ejemplo, si vamos a desarrollar una aplicación Web seguramente necesitaremos al menos los siguientes componentes: frontend, backend y alguna base de datos para almacenar la información.

Existen varias formas de representar el sistema en diagramas. Por ejemplo, utilizando un diagrama de componentes de UML, una aplicación Web se vería así:

<img src="/assets/images/software-architecture-components.png" alt="Componentes" class="photo border">
<p class="photo-description">Diagrama de Componentes</p>

También podríamos crear un diagrama de despliegue, que tendría detalles más específicos del diseño:

<img src="/assets/images/software-architecture-deployment.png" alt="Despliegue" class="photo border">
<p class="photo-description">Diagrama de Despliegue</p>

Parte importante del diseño del sistema es decidir qué tecnologías se van a usar dependiendo de los requerimientos funcionales y no funcionales de la aplicación.

Una buen diseño del sistema debe permitir modificar y evolucionar la aplicación en el tiempo. También debe permitir escalar rápidamente sin necesidad de realizar cambios en el código.

## El rol del arquitecto (líder técnico)

Hoy existen varias definiciones de un arquitecto de software, pero sería mejor cambiar el nombre del rol completamente. El arquitecto es, al final, un **líder técnico** que toma decisiones técnicas de una aplicación.

El líder técnico (arquitecto) es el responsable de definir el diseño del sistema (la arquitectura): qué componentes van a existir, cómo se van a comunicar esos componentes, y qué tecnologías se van a utilizar.

El líder técnico no necesariamente escribe código de la aplicación pero sí está involucrado en las discusiones y retos que los demás desarrolladores tengan. Adicionalmente, revisa código y aporta al proyecto haciendo los ajustes necesarios a medida que avanza el desarrollo.

Una parte importante del rol del líder técnico es estar constantemente probando nuevas tecnologías y compartiendo con el resto de la compañía los aprendizajes obtenidos.

Un buen líder técnico conoce y ha implementado diferentes estilos de arquitectura (microservicios, monolito, serverless, etc.), sabe cómo diseñar aplicaciones escalables, sabe cómo funcionan diferentes bases de datos (relacionales, documentales, llave-valor, etc.), cómo medir y monitorear los componentes a través de logs y herramientas de monitoreo (p.e. [NewRelic](https://newrelic.com/){:target="\_blank"}, [Datadog](https://www.datadoghq.com/){:target="\_blank"}, [Sentry](https://sentry.io/){:target="\_blank"} etc.).

## Patrones de diseño

Cuando se habla de arquitectura de software muchas personas lo asocian con patrones de diseño. Sin embargo, son dos conceptos diferentes.

La arquitectura (diseño del sistema) se refiere a la estructura de la aplicación y cómo se va a mover la información entre componentes. Los patrones de diseño son soluciones a problemas comunes que se repiten en el desarrollo de una aplicación.

Existen patrones de diseño para cada uno de los paradigmas de programación (orientado a objetos, funcional, etc.) y componentes de software (frontend, backend, acceso a datos, etc.).

Por ejemplo, en Programación Orientada a Objetos tenemos patrones como [Singleton](https://refactoring.guru/design-patterns/singleton){:target="\_blank"}, [Factory](https://refactoring.guru/design-patterns/factory-method){:target="\_blank"}, [Adapter](https://refactoring.guru/design-patterns/adapter){:target="\_blank"}, [Observer](https://refactoring.guru/design-patterns/observer){:target="\_blank"}, etc. En React existen patrones como el [HOC (Higher Order Component)](https://reactjs.org/docs/higher-order-components.html){:target="\_blank"}, [Hooks](https://reactjs.org/docs/hooks-intro.html){:target="\_blank"}, [Context](https://reactjs.org/docs/context.html){:target="\_blank"}, etc.

Los patrones de diseño también permiten una mejor comunicación entre desarrolladores, una especie de lenguaje en común.

## SOLID

Los [principios SOLID](https://es.wikipedia.org/wiki/SOLID) aplican principalmente al paradigma de programación orientada por objetos. Son 5 principios que corresponden a cada una de las letras:

- Single Responsability (Única Responsabilidad)
- Open Close (Abierto / Cerrado)
- Liskov Substitution (Sustitución de Liskov)
- Interface Segregation (Segregación de Interfaces)
- Dependency Inversion (Inversión de Dependencias)

Aunque algunas empresas aún preguntan por estos principios en las entrevistas, la realidad es que la mayoría no tiene mucho sentido cuando se está en ambiente más funcional como hoy ocurre con Node.js y React, por ejemplo.

Quizá el principio más importante es el primero: Única Responsabilidad. Este principio también aplica al paradigma funcional en el sentido que es recomendable que nuestras funciones, nuestros componentes, nuestros módulos tengan una única responsabilidad.

Otro principio importante es hacer nuestra aplicación modular, de modo que podamos cambiar ciertos componentes fácilmente, que es la idea detrás de la sustitución de Liskov (tercer principio). Por ejemplo, si nuestro proveedor de envío de correos es [Sendgrid](https://sendgrid.com/){:target="\_blank"} ¿Qué tan fácil es migrar a otro servicio como [Mailgun](https://www.mailgun.com/){:target="\_blank"} o [Postmark](https://postmarkapp.com/){:target="\_blank"}? ¿Qué tendríamos que cambiar?

## Conclusión

Aunque la metodología ágil no habla nada explícito sobre el diseño del sistema (arquitectura), eso no significa que debamos ignorarlo. Al contrario, un buen diseño es fundamental para el éxito de un proyecto.

Lo anterior no significa que debamos hacer una especificación completa del sistema antes de escribir nuestra primera línea de código, lo ideal es tener el diseño mínimo necesario para iniciar con la implementación, pero si deberíamos tener en cuenta al menos lo siguiente:

- Cuáles van a ser los componentes de nuestro sistema. Acá debemos listar también componentes externos como sistemas de almacenamiento (p.e. [Cloudinary](https://cloudinary.com/){:target="\_blank"}, [S3](https://aws.amazon.com/s3/){:target="\_blank"}, etc.), sistemas de correo ([Sendgrid](https://sendgrid.com/){:target="\_blank"}, [Mailgun](https://www.mailgun.com/){:target="\_blank"}, etc.), sistemas de pago, etc.
- Cuál va a ser la responsabilidad de cada componente que vamos a desarrollar y cómo es el flujo de comunicación entre los componentes.
- Cómo vamos a desplegar el sistema y cómo lo vamos a monitorear.
- Cómo va a escalar, qué va a pasar si la carga se incrementa.
- Qué tecnologías vamos a utilizar en cada uno de los componentes.

Estas decisiones las hace el líder técnico del proyecto, que debe ser alguien Senior con experiencia en el diseño de diferentes sistemas.

## Recursos adicionales

* [Complete guide to system design](https://www.educative.io/blog/complete-guide-to-system-design){:target="\_blank"}
* [Software architecture guide](https://martinfowler.com/architecture/){:target="\_blank"}
* [System Design Cheatsheet](https://gist.github.com/vasanthk/485d1c25737e8e72759f){:target="\_blank"}
* [System Design Interview Questions](https://www.freecodecamp.org/news/systems-design-for-interviews/){:target="\_blank"}
* [System Design Tutorial](https://systemdesigntutorial.com/){:target="\_blank"}

---

[^1]: Esto es debatible pero para efectos prácticos son lo mismo. Lo que estamos intentando es simplificar la terminología.