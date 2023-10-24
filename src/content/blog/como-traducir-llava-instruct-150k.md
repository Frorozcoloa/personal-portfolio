---
title: "¿Cómo traducir LLaVa-Instruct-150k?"
description: 'Un paso más para democratizar las IA en español'
pubDate: "2023-10-24T13:03:47.303Z"
heroImage: "https://raw.githubusercontent.com/Computer-Vision-in-the-Wild/CVinW_Readings/main/images/mfm_evolution.jpeg"
categories: ['Datasets']
tags: ['Deep Learning', 'Computer vision', 'NLP']
author: ['Frorozcol']
---
La IA multimodal cada vez la Inteligencia Artificial está más cerca de lograr el multimodal, simplemente tener un modelo que pueda usar cualquier tipo de dato, en este caso Openai lanzá su modelo [GPT-4V](https://openai.com/research/gpt-4v-system-card)

Pero no es el único modelo que se ha creado, también la comunidades Open Source, han creado también modelos largos multimodal (LMM). Al día de hoy han salido dos modelos [Fuyu-8B](https://www.adept.ai/blog/fuyu-8b) y [LLaVa](https://llava-vl.github.io)

Para ayudar a la democratización de inteligencia artificial, es importante recolectar datos de calidad para entrenar estos modelo.  Aquí te vengo a relatar mi trabecía traduciendo el dataset de [LLaVa-Instriuct-150k](https://huggingface.co/datasets/liuhaotian/LLaVA-Instruct-150K)

## ¿Cuál es el objectivo?

Todos los modelos anteriores fueron entrenados con datos en ingles y en el caso de GPT-4V, la mayoría de los datos son en ingles, lo que hace que el modelo empeoré cuando se use en un idioma diferente. Es por eso que si se quiere democratizar los LMM en español, es importante entrenar en este idioma.

## ¿Cómo es el dataset?

El dataset de [LLaVa-Instriuct-150k](https://huggingface.co/datasets/liuhaotian/LLaVA-Instruct-150K), está construido con [COCO ](https://cocodataset.org/#home)dataset. Lo que realizaron los investigadores fue, usar gpt-4V, donde le hacián varias preguntas de estas images. Veamos una de las filas.

Esta es la imagen que se utiliza

![Coco Image](https://storage.googleapis.com/kagglesdsdata/datasets/857191/1462296/coco2017/train2017/000000002374.jpg?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20231024%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20231024T150015Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=3a2c4bf3bb55bd20f24ca067e204825ec8c5312364e3cc9e1ac17252f7bcdb28fe7fda1b10735751a53afe34bb8546bd27342a9726172188baa47a7f807dcdad1cb290036c4a71e3435301451c046864973fd4fd39b3018c48ca9646cf9501ea8146208b1251a4f776bc04d6f4678a5373def2cf58f74585e5bae6e218e77572d3972fe7096f956c5ef931e80990624d3e1f9382089017e9ca21dc224b3953918d551dbf8d3f35b8456e81a16ede956a442f700d8a52b769f076cc45dd6dbbbbe641f00d99cb01825ee4ecf464b1d3e8fd20aad51bda56892360dcd6baa9b2606a0942ec9d894b9b4dc0e71af3af4e1bbda1dc3a03a97523d1b990816c0ac111 "id: 000000002374")

Este es el texto:

```json
[ 
{ "from": "human", "value": "<image>\nWhat could be the purpose of the sign on the pizza?" },
{ "from": "gpt", "value": "The purpose of the sign on the pizza could be an advertisement for a pizza restaurant or a promotional pamphlet. Featuring a cartoon mobster and placed directly on top of a pepperoni and cheese pizza, the sign likely aims to capture the attention of potential customers and showcase the pizza's quality. It seeks to entice potential customers by displaying a delicious-looking pizza and providing information about the restaurant or its offerings, effectively acting as a marketing tool for the business." } 
]
```

Podemos ver como se le hace una pregunta sobre la imagen y como hay un texto que es lo que ve GPT. Muchos de los datos es una conversación junto con la imagen.

## Reto

El reto es traducir, alrededor 150k datos, entendiendo que tambien dentro de cada fila hay más de un datos.

Para eso consideré lo siguiente:

### Primero: ¿Cómo traducir el dataset?

Lo primero que consideré fue en usar un modelo pre-entrenado. Para esto es necesario que la máquina tenga gpu y una buena Vram. Lo unico que podia tener estos servicios, es un servicio en la nube o  Google colaboratory. El problema es la cantidad de datos, lo que hace costoso la solución.

Afortunadamente existe una librería de python que usa [Googletrans](https://pypi.org/project/googletrans/), con esto ya no se necesita una máquina potente.

### Segundo: ¿Tamaño del dataset?

Siendo honesto, no se puede traducir el dataset en una corrida, es demasiado grande el dataset como para hacerlo. El error más común es de timeout, un problema muy común a la hora de usar servicios de internet. Así que decidí dividir el dataset en chucks de 500 datos. Todos estos se traducen y se guardan el dataset. Haciendo esto salen 315 chuck. El código por consola recibe el número de chunk, aunque esto se puede hacer por código, leyendo la carpeta donde se guarda el dataset. Por el momento solo lo dejé por consola.

### Tercero: ¿Cómo correrlo?

La primera prueba que hice, fue correrlo en mi computador. En un día logré sacar 67 chucks, un total de 31.500 datos traducidos. No había  llegado ni a la mitad de los chucks, por lo que bloquea mi computador y tampoco quiero dejarlo todo el día conectado.

Lo segundo que hice, es usar mi Jetson Nano, el problema es que no le dan soporte y el sistema operativo es desactualizado. La solución fue usar docker, creé un dockerfile y una imagen muy sencilla. Y ya con esto creado, lo subí a [Github](https://github.com/Frorozcoloa/traslated_llava_dataset), y lo descargué en la Jetson Nano. La conecté a internet y listo.

### Cuarto: Caidas por timeout

Esto por el momento si lo tengo de forma manual, no tengo en afán de traducir el datset, y por lo tanto, cada cierto tiempo reviso por ssh la ejecución del dockerfile, si veo que hay algún problema, lo vuelvo a levantar señalanado el chuck donde quedó.

## Verificación de la traducción

Recuerda que lo más importante es la calidad del dataset. El siguiente paso es que la comunidad corrija el dataset con [Argilla](https://argilla.io), ya pues las personas veran el texto original, traducido y la imagen. Con esto podrán hacer las correciones al dataset y mejorar la calidad de este.

La fiolosofía que tengo a la hora de crear estos dataset, es dejar una base estable que tiene un gran probabilidad de que funcione en muchos casos. Ya es trabajo de los cientificos de datos de buscar esas particularidades y corregirlas para tener una mejor calidad.
