---
layout: post
title:  "Serverless (FaaS)"
date:   2022-02-20 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/butterflies.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

Serverless es una nueva tendencia en el despliegue de aplicaciones que cada día adquiere más fuerza. En este post vamos a ver qué es, cómo funciona y cuál es la relación entre Serverless y FaaS (Functions as a Service).<!-- more -->

La forma tradicional de desplegar aplicaciones es alquilar un servidor (p.e. una máquina virtual de [AWS EC2](https://aws.amazon.com/ec2/){:target="\_blank"}) o un contenedor (p.e. [Heroku](https://heroku.com/){:target="\_blank"}) y pagar por horas o meses por tener nuestra aplicación disponible.

Una desventaja de este modelo es el desperdicio de recursos: si nuestra aplicación no está recibiendo tráfico o ejecutando alguna tarea estamos pagando por recursos que no están siendo utilizados. 

Otra desventaja es que si recibimos un pico de tráfico o de tareas vamos a tener que reaccionar para incrementar la capacidad o configurar alguna forma de escalamiento automático.

Serverless elimina estas desventajas. Por un lado, sólo pagamos por el tráfico o las tareas que ejecutemos y, por otro lado, el proveedor se encarga de escalar automáticamente la infraestructura a medida que incrementa el tráfico.

## ¿Qué es?

Serverless (“sin servidor” en Español) es un concepto muy amplio y un poco confuso, porque no es que no haya necesidad de un servidor, es que **nosotros no vamos a mantener esos servidores, alguien más lo va a hacer por nosotros**.

Bajo esa definición servicios como [Heroku](https://heroku.com/){:target="\_blank"}, [Firebase](https://firebase.google.com/){:target="\_blank"}, [S3](https://aws.amazon.com/s3/){:target="\_blank"} (el servicio de almacenamiento de AWS) o cualquier servicio de cómputo o almacenamiento donde no tengamos que mantener un servidor se podría considerar Serverless.

Dentro del mundo Serverless existen las **funciones como servicio** (en Inglés Functions as a Service o FaaS), que permiten ejecutar código (funciones) en la nube sin necesidad de mantener un servidor. En este post hablaremos principalmente de FaaS.

FaaS (Functions as a Service) empezó a ser popularizado en 2014 con el [lanzamiento de AWS Lambda](https://www.youtube.com/watch?v=9eHoyUVo-yg){:target="\_blank"}, un servicio de cómputo basado en eventos utilizando funciones en la nube. En los siguientes años [Google](https://cloud.google.com/functions){:target="\_blank"}, [Microsoft](https://azure.microsoft.com/en-us/services/functions/){:target="\_blank"}, [IBM](https://www.ibm.com/cloud/functions){:target="\_blank"} y otros lanzaron soluciones similares.

## ¿Cómo funciona?

La idea de FaaS es simple:

1. Escribimos una función en alguno de los lenguaje de programación que soporte el proveedor que vayamos a utilizar (p.e. [AWS Lambda](https://aws.amazon.com/lambda/){:target="\_blank"}, [Vercel](https://vercel.com/){:target="\_blank"}, [Netlify](https://netlify.com/){:target="\_blank"}, etc.).
2. Subimos nuestra función al proveedor.
3. En algunos proveedores (p.e. AWS Lambda) es necesario configurar un evento que va a disparar la función (p.e. una petición HTTP, la recepción de un email, almacenamiento en S3, etc.). En otros proveedores la única forma de invocar la función es a través de HTTP.

Y eso es todo, cada vez que ocurra el evento se va a ejecutar la función.

Por debajo, las soluciones FaaS hacen uso de contenedores para ejecutar las funciones.

## Casos de uso

FaaS es ideal para casos de uso como:

* El backend de aplicaciones.
* Tareas cortas esporádicas como envíos de correo, procesamiento de imágenes, generación dinámica de archivos, etc.
* Recepción y procesamiento de datos de dispositivos IoT.
* Webhooks. Reaccionar ante eventos que ocurren en otras aplicaciones o servicios.
* Creación de bots (de Slack o conversacionales).
* … entre otros!

## Limitaciones y desventajas

Antes de empezar con FaaS debes tener en cuenta que existen algunas limitaciones y desventajas de usar esta tecnología:

1. Tiempo máximo de ejecución. La mayoría de soluciones FaaS no están diseñadas para tareas que tomen más de unos minutos.
2. Concurrencia. Existen límites en el número de ejecuciones concurrentes de una función. Por ejemplo, AWS Lambda tiene un límite de 1000 ejecuciones concurrentes por defecto que se pueden incrementar a decenas de miles a través de una solicitud.
3. Capacidad de disco y memoria RAM. Depende del proveedor.
4. Tamaño de la función. Depende del proveedor.
5. Tamaño máximo de la petición y la respuesta. Depende del proveedor pero no está hecho para recibir archivos grandes (máximo algunos megabytes).
6. Número máximo de funciones desplegadas. Depende del proveedor.
7. Seguridad. Dependemos del proveedor para evitar ataques que generen costos para nosotros.
8. Precio. Generalmente se cobra por número de ejecuciones y/o tiempo de procesamiento, así que es necesario estimar estos valores para cada caso y cada proveedor. 

El último punto es particularmente importante porque el precio varía entre proveedores y está cambiando constantemente. La capa gratuita de [AWS Lambda](https://aws.amazon.com/lambda/){:target="\_blank"} es particularmente generosa al momento de escribir este post: 1 millón de ejecuciones y hasta 3.2 millones de segundos de ejecución al mes (dependiendo de la cantidad de memoria que se le asigne a cada función), pero es importante revisar los costos y compararlos con otras soluciones.

## Cómo empezar

Existen varios proveedores de FaaS. El más popular es [AWS Lambda](https://aws.amazon.com/lambda/){:target="\_blank"} pero no es el más fácil para empezar. Incluso muchas empresas utilizan [Serverless Framework](https://www.serverless.com/){:target="\_blank"} para desplegar sus funciones a [AWS Lambda](https://aws.amazon.com/lambda/){:target="\_blank"} y otros proveedores como [Microsoft Azure](https://azure.microsoft.com/en-us/services/functions/){:target="\_blank"}, [Google Cloud Platform](https://cloud.google.com/functions){:target="\_blank"}, entre otros.

Existen otros proveedores más pequeños, como [Vercel](https://vercel.com/){:target="\_blank"} y [Netlify](https://netlify.com/){:target="\_blank"}, que ofrecen alternativas más simples y fáciles de empezar. Para este ejemplo vamos a utilizar [Vercel](https://vercel.com/){:target="\_blank"}.

El primer paso es crear una cuenta en [Vercel](https://vercel.com/){:target="\_blank"}, puedes hacerlo utilizando email y contraseña o puedes utilizar Github como método de autenticación.

El siguiente paso es crear una carpeta llamada `ejemplo-faas`, una subcarpeta llamada `api` (por defecto Vercel toma las funciones de esta carpeta) y un archivo llamado `index.js`:

```
$ mkdir ejemplo-faas
$ cd ejemplo-faas
$ mkdir api
$ cd api
$ touch index.js
```

Abre el proyecto en tu editor preferido y agrega el siguiente contenido al archivo `index.js`:

```js
export default function handler(req, res) {
  res.status(200).end("Hello!")
}
```

Guarda el archivo, inicializa el repositorio de Git y crea un commit:

```
$ git init
$ git add .
$ git commit -m 'Versión Inicial'
```

Crea un [repositorio en Github](https://github.com/new) y hazle push con los siguientes comandos (también los vas a encontrar en las instrucciones del repositorio que crees en Github):

```
$ git remote add origin <url_al_repo>
$ git branch -M main
$ git push -u origin main
```

Vuelve a [Vercel](https://vercel.com/){:target="\_blank"} y agrega un nuevo proyecto desde el Dashboard:

<img src="/assets/images/vercel-1.jpg" alt="Dashboard de Vercel" class="photo border">
<p class="photo-description">El Dashboard de Vercel al momento de escribir este post. Puede que cambie pero debe existir la opción de crear un nuevo proyecto.</p>

Selecciona el repostorio que acabaste de crear y sigue las instrucciones para desplegarlo:

<img src="/assets/images/vercel-2.jpg" alt="Selección de proyecto en Vercel" class="photo border">
<p class="photo-description">Selección de proyecto en Vercel. Busca el proyecto de Github que creaste e impórtalo como muestra la flecha roja.</p>

En Vercel, en la página del proyecto, vas a encontrar la URL de la aplicación para probar tu función, recuerda agregar `api/index` al final de la URL para probar la función que creamos. Deberías ver el siguiente resultado:

<img src="/assets/images/vercel-3.jpg" alt="Resultado del llamado a la función" class="photo border">
<p class="photo-description">Resultado del llamado a la función desde el navegador.</p>

¡Felicitaciones, acabas de desplegar tu primera función en la nube! No parece muy diferente a desplegar una aplicación Web normal, pero la diferencia es que sólo vas a pagar por el uso real de tu aplicación y no necesitas preocuparte por la escalabilidad (aunque es bueno estar pendiente de los costos).