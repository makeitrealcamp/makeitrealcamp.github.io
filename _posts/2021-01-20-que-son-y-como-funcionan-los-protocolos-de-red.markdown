---
layout: post
title:  "¿Qué son y cómo funcionan los protocolos de red?"
date:   2021-01-20 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/protocolo-de-red.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

Así como los humanos utilizamos protocolos para facilitar la comunicación, los computadores necesitan reglas específicas para comunicarse entre ellos. En este post vamos a ver qué son, cómo funcionan y cómo crear nuestro propio protocolo!<!-- more -->

Un **protocolo de red** no es más que un documento que define las reglas y la estructura de los mensajes que se van a intercambiar entre dos o más computadores. A estos documentos se les conoce como RFC (Request For Comments)[^1].

Quizá hayas escuchado de protocolos como [HTTP (HyperText Transfer Protocol)](/el-protocolo-http/){:target="\blank"}, que se utiliza cada vez que abres una página desde un navegador, o [SMTP (Simple Mail Transfer Protocol)](https://es.wikipedia.org/wiki/Protocolo_para_transferencia_simple_de_correo){:target="\_blank"}, que se utiliza para enviar y recibir correos electrónicos. Estos protocolos son abiertos, así que puedes consultar los documentos (RFC) que los describen en [este](https://tools.ietf.org/html/rfc2616){:target="\_blank"} y [este](https://tools.ietf.org/html/rfc5321){:target="\_blank"} enlace respectivamente.

HTTP y SMTP son protocolos de alto nivel. Los mensajes de estos protocolos viajan sobre otros protocolos de más bajo nivel como [TCP (Transfer Control Protocol)](https://es.wikipedia.org/wiki/Protocolo_de_control_de_transmisi%C3%B3n){:target="\_blank"} e [IP (Internet Protocol)](https://es.wikipedia.org/wiki/Protocolo_de_internet){:target="\_blank"}, que a su vez viajan sobre otros protocolos como [Ethernet](https://es.wikipedia.org/wiki/Ethernet){:target="\_blank"} (para redes cableadas) o [WiFi](https://es.wikipedia.org/wiki/Wifi){:target="\_blank"} (redes inalámbricas)[^2].

La siguiente animación muestra un ejemplo de cómo se encapsulan los mensajes de un protocolo en mensajes de otro protocolo de más bajo nivel para enviar enviar información de un origen a un destino (en este caso se va a enviar un fragmento de código HTML):

<img src="/assets/images/network-layers.gif" alt="Capas de Red" class="photo border">
<p class="photo-description">Los mensajes de un protocolo se transmiten dentro de los mensajes de protocolos de más bajo nivel.</p>

El código HTML se encapsula en un mensaje HTTP, que es un protocolo de **la capa de aplicación**. El mensaje HTTP se encapsula en un mensaje TCP, que es un protocolo de **la capa de transporte**. El mensaje TCP se encapsula en un mensaje IP, que es un protocolo de **la capa de Internet**. Por último, el mensaje IP se encapsula en el protocolo de red (p.e. alguno de los protocolos Wifi) y se envía al destino.

Cuando el mensaje llega al destino ocurre el proceso contrario: la capa de red recibe el mensaje y le pasa el cuerpo (el mensaje IP) a la capa de Internet, que a su vez le pasa el cuerpo (el mensaje TCP) a la capa de transporte, y así sucesivamente.

Si el mensaje del protocolo de alto nivel es muy largo, es posible que la capa de transporte (en este caso TCP) divida el mensaje y lo envíe por partes, para después ensamblarlo en el destino.

La mayoría de mensajes de un protocolo tienen un encabezado y (opcionalmente) un cuerpo. Por ejemplo, un mensaje de **respuesta HTTP** incluye la versión del protocolo, el código de respuesta (p.e. 200 OK o 404 Not Found), información adicional como el tipo de contenido que va en el cuerpo, y la longitud del cuerpo, entre otros:

```http
HTTP/1.1 200 OK
Server: wikipedia.org
Content-Type: text/html
Content-Lenght: 2026

<h1>Hola Mundo</h1>
```

**Nota:** El protocolo HTTP es el protocolo más importante que debería conocer todo desarrollador Web. Si quieres aprender más de este protocolo te recomiendo [este post del blog de Make it Real](/el-protocolo-http/){:target="\_blank"}.

Esta arquitectura en capas brinda mucha flexibilidad porque cada capa se encarga de una parte específica de la comunicación. Los protocolos de alto nivel no se preocupan de cómo se transportan sus mensajes, y los protocolos de bajo nivel no se preocupan de la información que están transportando.

A los mensajes también se les conoce con el nombre de **paquetes**.

## Creemos nuestro propio protocolo
Vamos a crear un protocolo muy simple que permita a dos computadores (un cliente y un servidor) saludarse y despedirse. Las características de nuestro protocolo van a ser las siguientes:

* Va a ser de alto nivel (de aplicación).
* Va a ser de petición-respuesta, es decir que el cliente envía un mensaje y el servidor responde con un mensaje de vuelta.
* Va a ser un protocolo sin estado, es decir, que cada mensaje va a ser independiente del anterior.

Las reglas de nuestro protocolo son las siguientes:

* Cuando el cliente envía el texto `HOLA`, el servidor responderá con el texto `BUENAS`.
* Cuando el cliente envíe el texto `CHAO`, el servidor responderá con el texto `ADIOS` y cerrará la conexión.
* Si el cliente envía un texto diferente a los anteriores el servidor responderá con el texto `NO ENTIENDO`.

¡Eso es todo! Cualquier persona puede implementar un cliente o un servidor de este protocolo en cualquier lenguaje de programación. Vamos a implementar un servidor en [Node.js](https://nodejs.org/){:target="\_blank"} que implemente este protocolo.

```javascript
const net = require('net')

const server = net.createServer(socket => {
	socket.on('data', data => {
    // convertimos la data en texto y quitamos el salto de línea
    const message = data.toString('utf8').replace(/[\r\n]+/g, "")
    if (message === "HOLA") {
      socket.write("BUENAS\n\r")
    } else if (message === "CHAO") {
      socket.write("ADIOS\n\r")
      socket.close()
    } else {
      socket.write("NO ENTIENDO\n\r")
    }
  })
})

server.listen(1337, '127.0.0.1')
```

Para crear nuestro servidor en Node.js requerimos primero el módulo `net`, que nos permite interactuar con la capa de transporte (TCP). Después creamos el servidor que recibe conexiones a través de **sockets** por los que vamos a recibir y enviar los mensajes de nuestro protocolo. Por último prendemos el servidor, que se va a quedar escuchando las conexiones que lleguen al puerto `1337`.

**Nota:** Un **socket** es una combinación de una IP y un **puerto** (en este caso el puerto `1337`). Los **puertos** no son más que divisiones lógicas en la red que permiten a los computadores conectarse con más de un computador/servidor a la vez.

Para iniciar el servidor ejecutamos el comando `node` seguido del nombre del archivo, por ejemplo:

```shell
node index.js
```

Para detenerlo debes oprimir `Ctrl+C`.

Para probar el servidor vamos a conectarnos a través de [Telnet](https://es.wikipedia.org/wiki/Telnet){:target="\_blank"}, que es una aplicación de la línea de comandos que va a abrir una conexión TCP al host y puerto que especifiquemos en los argumentos. En este caso nos vamos a conectar a nuestro propio computador, al puerto 1337:

```shell
telnet localhost 1337
```

Si te aparece un mensaje de error diciendo que el comando no fue encontrado deberás primero instalarlo para tu sistema operativo[^3]. De lo contrario deberías ver algo similar a lo siguiente:

```shell
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
```

Ahora podemos enviar algunos mensajes y ver las respuestas. Por ejemplo:

```shell
HOLA
BUENAS
HELLO
NO ENTIENDO
CHAO
ADIOS
Connection closed by foreign host.
```

Podemos seguir extendiendo nuestro protocolo con otros tipos de mensajes y organizar esta información en un documento para compartirlo con el mundo, y que otras personas puedan crear servidores o clientes a partir de nuestro protocolo :)

## Conclusión

Puedes pensar en los protocolos de red como las aplicaciones de Internet. Además de TCP e IP, que se utilizan para enrutar los mensajes de protocolos de más alto nivel, el primer protocolo que impulsó el crecimiento de Internet fue SMTP (correo electrónico).

Sin embargo, no fue sino hasta principios de los 90's, con la creación del protocolo HTTP y la creación de navegadores gráficos que realmente se disparó la adopción. Desafortunadamente, fue un crecimiento muy rápido y descontrolado, lo que ocasionó la burbuja del año 2000 (también conocida como la [burbuja del punto com](https://es.wikipedia.org/wiki/Burbuja_puntocom){:target="\_blank"}), pero con el tiempo el mercado fue madurando y hoy tenemos todo tipo de aplicaciones interesantes sobre esta increíble tecnología que es Internet.  

---

[^1]: El nombre es un remanente histórico. Antes estos documentos eran literal un solicitud de comentarios, pero hoy son documentos oficiales que estandarizan los protocolos.

[^2]: Ethernet y WiFi no son protocolos como tal, pero se usan estos términos para hablar del conjunto de protocolos que tienen el mismo fin.

[^3]: Puedes hacer una búsqueda en Internet de "telnet" seguido de tu sistema operativo, por ejemplo "telnet windows".
