---
layout: post
title:  "Cómo incluir ChatGPT en tus aplicaciones"
date:   2023-05-01 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/openai-chatgpt.png
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

ChatGPT cuenta con un API que nos permite agregar, a nuestras aplicaciones Web y móviles, funcionalidades como chatbots, análisis y extracción de texto, creación de resúmenes, explicación de código, entre muchas otras. En este post vamos a ver cómo usar este poderoso API desde **Node.js**.<!-- more -->

Para empezar necesitas crear una cuenta en [OpenAI](https://openai.com/){:target="\_blank"} y obtener el [API key](https://beta.openai.com/account/api-keys){:target="\_blank"}. Al registrarte recibirás automáticamente créditos gratuitos para hacer pruebas[^1].

El siguiente paso es instalar el [paquete de OpenAI](https://www.npmjs.com/package/openai){:target="\_blank"} usando **npm**. Ejecuta el siguiente comando en la consola:

```
npm install openai
```

Crea ahora un archivo llamado `index.js` con el siguiente código de prueba:

```js
const { Configuration, OpenAIApi } = require("openai");

const configuration = new Configuration({
  apiKey: "API-KEY",
});
const openai = new OpenAIApi(configuration);

openai.createChatCompletion({
  model: "gpt-3.5-turbo",
  messages: [{ role: "user", content: "Hello world" }],
  max_tokens: 100
}).then(completion => {
  console.log(completion)
  console.log(completion.data.choices[0].message.content);
});
```

**Nota:** No olvides reemplazar el texto `API-KEY` con el API key que obtuviste en el primer paso.

Lo primero que estamos haciendo es requerir la librería y crear una configuración con el API key para instanciar el API (`OpenAIApi`). Después utilizamos el método `createChatCompletion` que hace el llamado al API y retorna un objeto de resultados similar al siguiente:

```js
{
  id: "chatcmpl-123",
  object: "chat.completion",
  created: 1677652288,
  choices: [{
    index: 0,
    message: {
      role: "assistant",
      content: "\n\nHello there, how may I assist you today?",
    },
    finish_reason: "stop"
  }],
  usage: {
    prompt_tokens: 9,
    completion_tokens: 12,
    total_tokens: 21
  }
}
```

Las respuestas vienen en la llave `choices`. Por defecto sólo llega una respuesta pero podemos pedir más. Para ver todas las opciones que recibe `createChatCompletion` [consulta la documentación en este link](https://platform.openai.com/docs/api-reference/chat/create){:target="\_blank"}.

¡Eso es todo! El límite está en tu imaginación.

Ten en cuenta que una vez consumas tus créditos deberás ingresar una tarjeta de crédito para poder continuar usando el servicio. Consulta la [página de precios](https://openai.com/pricing#language-models){:target="\_blank"} para más información.



---

[^1]: Al momento de escribir este post son $5 dólares que expiran en 3 meses.