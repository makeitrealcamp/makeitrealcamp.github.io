---
layout: post
title:  "Introducción al Aprendizaje Automático (Machine Learning)"
date:   2020-10-09 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/data-points.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

El **Aprendizaje Automático** está en todas partes: filtra correos en tu bandeja de entrada, te sugiere artículos en Amazon y películas en Netflix, etiqueta tus fotos de Facebook, identifica tu voz en Siri, identifica frases en la búsqueda de Google y detecta fraudes financieros, por nombrar algunas aplicaciones.<!-- more -->

Y este es sólo el comienzo. Eventualmente el Aprendizaje Automático conducirá nuestros autos, identificará y prevendrá enfermedades, será nuestro asistente personal en las tareas del hogar, entre otras aplicaciones que aún no nos imaginamos.

Pero no nos adelantemos. Veamos qué es el Aprendizaje Automático y cómo funciona.

El Aprendizaje Automático es una técnica que nos permite predecir, identificar o realizar sugerencias con la ayuda de computadoras.

La forma en que funciona el Aprendizaje Automático es la siguiente: **a partir de información existente se utiliza un algoritmo para entrenar un programa que va a aprender a predecir, identificar o sugerir algo nuevo**.

Generalmente, los proyectos de Aprendizaje Automático se componen de los siguientes pasos:

1. Preparar la información.
2. Seleccionar el algoritmo de entrenamiento.
3. Evaluar los resultados.

### 1. Preparar de la información

Todo proyecto de Aprendizaje Automático requiere información preexistente. Esa información puede salir de los datos de una empresa, organización o datos históricos, entre otros.

El siguiente paso es cargar, visualizar y limpiar la información. Puede que la información no esté en el formato que queremos o que la información no esté completa.  Además tenemos que seleccionar los datos que son relevantes para nuestro modelo y muchas veces vamos a necesitar manipular esos datos para que sean consistentes (p.e. pasar todo el texto a minúsculas o normalizar las opciones).

### 2. Seleccionar el algoritmo de entrenamiento

Los algoritmos de entrenamiento se pueden clasificar en dos grandes categorías: aprendizaje supervisado y no supervisado.

En el **aprendizaje supervisado** utilizamos la información preexistente para hacer predicciones de nueva información. Por ejemplo, podemos predecir la necesidad de personal de un restaurante basados en información histórica.

Ejemplos de algoritmos en esta categoría incluyen: Regression Analysis, Support Vector Machine, Decision Tree, Random Forest y Neural Networks, entre otros.

En el **aprendizaje no supervisado** queremos encontrar patrones ocultos que se encuentren en la información preexistente. Por ejemplo, entender cuáles son los productos de una tienda que los clientes compran al tiempo o encontrar segmentos de clientes.

Ejemplos de algoritmos en esta categoría incluyen: k-Means Clustering, Association Rules y Principal Component Analysis, entre otros.

Cada algoritmo tiene parámetros que se pueden ajustar para buscar mejores resultados.

### 3. Evaluar los resultados

Después de construir el modelo, es importante evaluarlo. Existen varias métricas que nos pueden ayudar dependiendo de lo que estemos midiendo.

Por ejemplo, si estamos intentando predecir si una persona se va a registrar en una página, podemos calcular la precisión como la proporción de predicciones que fueron correctas.

Por otro lado, si estamos intentado predecir el precio de un apartamento una mejor métrica es la raíz del error cuadrátrico medio., que no es más que una forma de medir qué tan cerca nuestro modelo estuvo en las predicciones.

## ¿Por dónde empezar?

Aunque es posible implementar **Aprendizaje Automático** en cualquier lenguaje de programación, Python es el lenguaje más usado debido a que es relativamente fácil de aprender y tiene excelentes librerías como [Numpy](https://numpy.org/){:target="\_blank"}, [Pandas](https://pandas.pydata.org/){:target="\_blank"}, [scikit-learn](https://scikit-learn.org/stable/){:target="\_blank"} y [Matplotlib](https://matplotlib.org/){:target="\_blank"}, entre otras.

Adicionalmente, existe un ambiente de ejecución llamado [Jupyter Notebook](https://jupyter.org/){:target="\_blank"} que permite crear documentos con código, texto y visualizaciones. Es una forma fácil y rápida de empezar de experimentar con Python y el Aprendizaje Automático en general.

En Make It Real tenemos un [programa de Data Science](https://makeitreal.camp/data-science-online?utm_source=blog&utm_medium=web&utm_campaign=inbound) en donde aprenderás los conceptos, técnicas y librerías que te ayudarán a ver los datos de una forma diferente de la mano de mentores experimentados. Te invitamos a conocerlo!
