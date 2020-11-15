---
layout: post
title:  "Github Actions (Video)"
date:   2020-11-15 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/gears.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

En este post (y video) vamos a ver cómo automatizar tareas del ciclo de desarrollo de software utilizando [Github Actions](https://github.com/features/actions){:target="\_blank"}.<!-- more -->

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/IivR-beDCS0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

Con [Github Actions](https://github.com/features/actions){:target="\_blank"} podemos automatizar tareas como empaquetamiento, ejecución de pruebas y despliegue de aplicaciones, entre muchas otras.

[Github Actions](https://github.com/features/actions){:target="\_blank"} está disponible en todos los repositorios de Github y es similar a otras herramientas de integración continua y automatización como [Jenkins](https://www.jenkins.io/){:target="\_blank"}, [TravisCI](https://travis-ci.org/){:target="\_blank"}, [CircleCI](https://circleci.com/){:target="\_blank"} o [Azure Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines/){:target="\_blank"}.

En mi opinión, la ventaja principal de [Github Actions](https://github.com/features/actions){:target="\_blank"} es que está integrado directamente a Github y puedes ejecutar tareas automatizadas cuando ocurren eventos en tus repositorios (p.e. al hacer push, al dejar un comentario, al abrir un PR, eliminar un issue, etc.).

El concepto principal de [Github Actions](https://github.com/features/actions){:target="\_blank"} son los **workflows (flujos de trabajo)**. Un **workflow** está compuesto de unos **triggers** que determinan cuándo se va a ejecutar el workflow, y unos **jobs**, que definen qué va a hacer el workflow.

Los **workflows** se ejecutan en máquinas virtuales. El plan gratuito de Github ofrece 2000 minutos de ejecución al mes, que es más que suficiente para proyectos y equipos pequeños. Si necesitas más tiempo deberás pagar por los minutos adicionales.

Los **workflows** se definen en un archivo en formato YAML, que es similar a JSON pero con una sintaxis diferente: usa indentación en vez de corchetes (`{}`). Veamos un ejemplo:

```yaml
name: Hello Workflow

on: workflow_dispatch

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello World!"
```

Las definiciones de los workflows se guardan en archivos con extensión `.yml` o `.yaml` en la carpeta `.github/workflows/` del repositorio. El archivo puede tener cualquier nombre.

El anterior fue un ejemplo muy simple (y poco útil) de un **workflow** con un único **job** que imprime “Hello World” en la consola.

La llave `name` define el nombre del **workflow**, que aparecerá en Github, en la pestaña Actions del repositorio.

En la llave `on` van los **triggers** que dispararán el workflow, en este caso utilizamos `workflow_dispatch`, que agrega un botón en la interfaz de Github para ejecutarlo manualmente. Pero [hay muchos eventos](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows){:target="\_blank"} que puedes utilizar para disparar tu **workflow**. Incluso puedes programarlos para que se ejecuten en una fecha o intervalo de tiempo.

En la llave `jobs` va lo que queremos que pase cuando se dispare el workflow. En este caso tenemos un job llamado `hello` que utiliza una máquina virtual basada en Ubuntu y que imprime “Hello World!” en la consola.

Este workflow lo debemos subir al repositorio en un commit y podemos ejecutarlo desde la interfaz Web de Github:

<img src="/assets/images/github-workflow-dispatch.jpg" alt="Running Github Workflow manually" class="photo border">

La ejecución se ve de la siguiente forma:

<img src="/assets/images/github-workflow-run.jpg" alt="Output of a Worflow run" class="photo border">

En este caso sólo tenemos un **job** llamado `hello`, pero si tuvieramos más de uno debes tener en cuenta que los **jobs** se corren en paralelo. En el video puedes ver un ejemplo de esto y cómo crear dependencias entre **jobs**.

Si la ejecución falla verás un mensaje de error y recibirás un correo con los detalles, que es muy útil cuando tienes **workflows** que ejecutan pruebas o despliegan en producción.

## Actions

Pero lo interesante de Github Actions es que podemos reutilizar recetas para realizar tareas comunes como hacer checkout del repositorio, configurar el ambiente (Node.js, Ruby, etc.), verificar la calidad del código, desplegar en Heroku, etc. Existe un [marketplace completo de Actions](https://github.com/marketplace?type=actions){:target="\_blank"} que puedes usar en tus **workflows**.

Veamos un ejemplo que usa un **action** para descargar el código ([actions/checkout](https://github.com/actions/checkout){:target="\_blank"}), otro action para configurar Node.js ([actions/setup-node](https://github.com/actions/setup-node){:target="\_blank"}) y otro action para desplegar en Heroku ([akhileshns/heroku-deploy](https://github.com/AkhileshNS/heroku-deploy){:target="\_blank"}):

{% raw %}
```yaml
name: Run tests and deploy

on:
  push:
    branches: master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node_version: '14'
      - run: npm install
      - run: npm test
      - uses: akhileshns/heroku-deploy
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: "..."
          heroku_email: "..."
```
{% endraw %}

Si vas a utilizar información sensible en tus **workflows** (como en este caso la llave del API de Heroku) es mejor crear un **secret**, que puedes encontrar en la configuración del repositorio (settings) como se muestra en la siguiente imagen:

<img src="/assets/images/github-secrets.jpg" alt="Github Secrets" class="phot border">

Y una vez que hayas creado el **secret**, lo puedes utilizar en tus **workflows** de la siguiente forma reemplazando `HEROKU_API_KEY` con el nombre de tu **secret**:

{% raw %}
```yaml
${{ secrets.HEROKU_API_KEY }}
```
{% endraw %}

---

[Github Actions](https://github.com/features/actions){:target="\_blank"} es una poderosa y versátil herramienta que te va a permitir automatizar gran parte de tu ciclo de desarrollo de software. En Make It Real lo utilizamos en varios proyectos. De hecho, este blog se despliega automáticamente con un **workflow** cada vez que publicamos o hacemos cambios a un post.

Espero que este post te haya dado una idea de lo que puedes hacer con [Github Actions](https://github.com/features/actions){:target="\_blank"} y te invito a ver el video de acompañamiento en el que encontrarás más información de esta interesante herramienta.
