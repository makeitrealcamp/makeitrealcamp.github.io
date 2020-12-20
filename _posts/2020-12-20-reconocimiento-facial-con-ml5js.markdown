---
layout: post
title:  "Reconocimiento facial con ml5.js"
date:   2020-12-20 02:00:00 -0500
author: Germán Escobar
image: /assets/images/bg-images/reconocimiento-facial-monalisa.jpg
gravatar: //www.gravatar.com/avatar/12270acfe9b6842e1a5b6e594382f149.jpg?s=80
---

En este post vamos a utilizar [p5.js](https://p5js.org/){:target="\_blank"} y una librería de Machine Learning llamada [ml5.js](https://ml5js.org/){:target="\_blank"} para implementar una solución de reconocimiento facial en el navegador!<!-- more -->

La solución aprenderá a identificar personas por su nombre:

<img src="/assets/images/face-recognition.gif" alt="Face Recognition" class="photo border">
<p class="photo-description">La solución final me reconoce correctamente! A la derecha está el resultado del entrenamiento a la red neuronal.</p>

Los pasos que vamos a implementar en este tutorial son los siguientes:

1. Mostrar el video capturado por la cámara en la pantalla utilizando [p5.js](https://p5js.org/){:target="\_blank"}.
2. Detectar las caras en el video utilizando [ml5.js](https://ml5js.org/){:target="\_blank"}.
3. Entrenar un clasificador facial (una red neuronal) para que identifique el nombre de la persona.

Puedes seguir cada paso de este tutorial en un [editor de p5.js](https://editor.p5js.org/){:target="\_blank"} o puedes ver y ejecutar el código final [en este enlace](https://editor.p5js.org/germanescobar/sketches/sEc369bfv){:target="\_blank"}.

## 1. Mostrar el video de la cámara

Vamos a modificar el código inicial de [p5.js](https://p5js.org/){:target="\_blank"} para capturar el video de la cámara utilizando la función `createCapture` y después lo mostramos en el navegador con la función `image`:

```javascript
let video
function setup() {
  createCanvas(900, 520)
  video = createCapture(VIDEO)
  video.size(640, 520)
  video.hide()
}

function draw() {
  image(video, 0, 0)
}
```

**Nota:** Si aún no conoces [p5.js](https://p5js.org/){:target="\_blank"} te recomendamos [ver este post](/practicando-programacion-con-p5js/){:target="\_blank"} primero.

Al ejecutar este código deberías ver el video de la cámara en el lienzo de p5.js (es posible que primero te pida permisos para acceder a la cámara).

## 2. Detección Facial

El siguiente paso es detectar las caras en el video y dibujar un rectángulo alrededor de ellas. Para eso debes abrir el archivo `index.html` y agregar la librería de [ml5.js](https://ml5js.org/){:target="\_blank"}:

```html
<script src="https://unpkg.com/ml5@0.6.0/dist/ml5.js" type="text/javascript"></script>
```

Volvamos a `sketch.js`  y escribamos el código que va a identificar las caras:

```javascript
let detector
const imgSize = 64
function setup() {
  // ...
  const detectionOptions = {
    withLandmarks: true,
    withDescriptors: false
  };

  detector = ml5.faceApi(video, detectionOptions, () => {
    console.log('Face API Model Loaded!')
    detectFace()
  })
}
```

La función `detectFace` es la que va a hacer la detección facial utilizando el método `detect` del `detector` que inicializamos en el código anterior:

```javascript
let boxes = []
function detectFace() {
  detector.detect((err, detections) => {
    if (err) return console.error(err)

    if (detections && detections.length > 0) {
      // caras detectadas!!
      boxes = createBoxes(detections)
    }

    // llamamos a este método continuamente
    detectFace()
  })
}
```

Este código utiliza una función llamada `createBoxes` que va a crear un arreglo de cajas:

```javascript
function createBoxes(detections) {
  const boxes = []
  for(let i = 0; i < detections.length; i++){
    const alignedRect = detections[i].alignedRect;

    const box = {
      x: alignedRect._box._x,
      y: alignedRect._box._y,
      width: alignedRect._box._width,
      height: alignedRect._box._height,
      label: ""
    }
    boxes.push(box)
  }

  return boxes
}
```

Por cada cara detectada el método `createBoxes` crea un objeto con las coordenadas y longitudes que después vamos a utilizar en el método `draw`. Fíjate que también hay una llave `label` que después vamos a utilizar para guardar el nombre de la persona que está en esa caja.

Modifiquemos el método `draw` para pintar las cajas:

```javascript
function draw() {
  // ...
  drawBoxes()
}

function drawBoxes() {
  for (let i=0; i < boxes.length; i++) {
    const box = boxes[i]
    noFill()
    stroke(161, 95, 251)
    strokeWeight(4)
    rect(box.x, box.y, box.width, box.height)

    strokeWeight(2)
    textSize(21)
    textAlign(CENTER)
    text(box.label, box.x + (box.width / 2), box.y + box.height + 20)
  }
}
```

SI ejecutas el código hasta acá deberías ver un rectángulo sobre cada cara que se encuentre en la pantalla:


Puedes también consultar el código completo hasta este punto [en este enlace](https://editor.p5js.org/germanescobar/sketches/7PMaiV2Ha){:target="\_blank"}.

## 3. Clasificación Facial

Vamos a entrenar una red neuronal para que clasifique a las personas por nombre. Este proceso se divide en 3 pasos:

1. Tomar varias fotos de cada persona y agregarlas al clasificador con su respectiva etiqueta (el nombre de la persona).
2. Entrenar el modelo.
3. Clasificar!

Primero inicialicemos el clasificador en el método `setup`:

```javascript
let classifier
function setup() {
  // ...

  let options = {
    inputs: [imgSize, imgSize, 4],
    task: 'imageClassification',
    debug: true,
  }
  classifier = ml5.neuralNetwork(options)
}
```

Ok, ahora necesitamos un campo de entrada y un botón para capturar y etiquetar las fotos. Hagamos eso:

```javascript
let nameInput
function draw() {
  // ...
  drawInputs()
}

function drawInputs() {
  nameInput = createInput('');
  nameInput.position(650, 30)

  const takePhotoBtn = createButton("Capture")
  takePhotoBtn.position(810, 30)
  takePhotoBtn.mousePressed(() => addExample(nameInput.value()))
  }
```

Cuando el usuario hace click sobre el botón estamos llamando una función `addExample` que agrega la foto con el nombre al clasificador:

```javascript
function addExample(label) {
  if (boxes.length === 0) {
    console.error("No face found")
  } else if (boxes.length === 1) {
    const box = boxes[0]
    img = get(box.x, box.y, box.width, box.height)
    img.resize(imgSize, imgSize)
    console.log('Adding example: ' + label);
    classifier.addData({ image: img }, { label });
  } else {
    console.error("Only one face should appear")
  }
}
```

Este código utiliza las cajas que creamos al detectar las caras. Primero valida que sólo haya una en el video y, si es así, le ajusta el tamaño y la agrega al clasificador.

Agreguemos un botón a la función `drawInputs` para entrenar el modelo:

```javascript
let trained = false
function drawInputs() {
  // ...
  const trainBtn = createButton("Train")
  trainBtn.position(650, 80)
  trainBtn.mousePressed(() => {
    trainModel()
  })
}
```

La función `trainModel` es la siguiente:

```javascript
function trainModel() {
  classifier.normalizeData();
  classifier.train({ epochs: 50 }, () =>  {
    console.log('training complete')
    trained = true
  })
}
```

Por último, vamos a modificar la función `detectFace` para que clasifique las caras cuando el modelo ya esté entrenado:

```javascript
function detectFace() {
  faceapi.detect((err, results) => {
    if (err) return console.error(err)

    if (results && results.length > 0) {
      boxes = getBoxes(results)
      if (trained) {
        for (let i=0; i < boxes.length; i++) {
          const box = boxes[0]
          classifyFace(box)
        }
      }
    }
    detectFace()
  })
}
```

Necesitas al menos dos personas, y tomar mínimo 10 fotos de cada una para entrenar el modelo. Puedes ver y probar el código hasta este punto [en este enlace](https://editor.p5js.org/germanescobar/sketches/7PMaiV2Ha){:target="\_blank"}.

## Algunas mejoras

Hasta ahora, siempre que ejecutamos el código debemos tomar las fotos y entrenar el modelo. Para evitar esto tenemos dos opciones:

1. Guardar la información de las fotos en un JSON. Cada vez que ejecutamos el código las cargamos y volvemos a entrenar el modelo.
2. Guardar el modelo ya entrenado.

Afortunadamente ml5.js incluye funciones para hacer todas estas operaciones. Podemos guardar la información de las fotos en un archivo JSON con `saveData` y volverlo a cargar con `loadData`. Agreguemos botones en el método `drawInputs` para esto:

```javascript
function drawInputs() {
  // ...

  // botón para guardar las fotos en un JSON
  const saveDataBtn = createButton("Save Data")
  saveDataBtn.position(650, 180)
  saveDataBtn.mousePressed(() => classifier.saveData('model'))

  // botón para cargar las fotos de un JSON
  const loadDataInput = createFileInput(file => {
    classifier.loadData([file.file], () => console.log("Data Loaded"));
  })
  loadDataInput.position(650, 130)  
}
```

También existen las funciones `save` y `load` para guardar y cargar el modelo ya entrenado que puedes [consultar en la documentación](https://learn.ml5js.org/#/reference/neural-network?id=save){:target="\_blank"}.

---

En este tutorial vimos cómo utilizar dos modelos de Machine Learning. El primero es el que utilizamos para identificar las caras en el video que ya viene incluido en [ml5.js](https://ml5js.org/){:target="\_blank"}. Existen otros modelos ya entrenados que te permiten identificar poses, detectar objetos, clasificar sonidos e identificar el sentimiento de un texto, entre otros que puedes consultar en la documentación!

El otro modelo fue una red neuronal que debemos entrenar capturando imágenes del video y etiquetándolas con nombres de personas. Inténtalo con tus amigos y/o familiares y cuéntanos cómo funciona!
