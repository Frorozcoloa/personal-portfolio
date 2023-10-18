---
title: "¿Por qué recolectar datos de NLP en español?"
description: "Los LLMs no hablan en español y están sesgados"
pubDate: "2023-10-18T03:14:24.052Z"
heroImage: "https://cdn.pixabay.com/photo/2015/05/10/20/09/spanish-761512_1280.jpg"
categories: ["Tecnología"]
tags: ["Tecnología", "ciencia de datos", "Inteligencia artificial", "NLP", "Español"]
author: ["Fredy Orozco"]
---

¿Has probado Chatgpt, midjourney o stable diffusion? Si la
respuesta es sí, déjame de cierte que hay un gran problema, al utilizar algún
modelo de estos o algún modelo grande del lenguaje (LLM) en un idioma diferente
al inglés, se pierde alrededor 32% de la precisión del modelo. Todo esto se
debe a que los LLM fueron entrenados con grandes conjuntos de datos en inglés y
muy pocos datos en otro idioma. Si miramos el total de dataset en Hugging Face encontramos
solo 5% de total de dataset están en español. Siendo el español por mucho el cuarto
idioma con más datasets y modelos en esta plataforma, solo por encima el inglés,
chino y el francés.

Esto es un gran problema para la democratización de la
Inteligencia artificial. El español es el segundo idioma más hablado en el
mundo, el 20% de la población mundial habla español y no hay datos para que
LLMs y otros modelos, aprenda la riqueza del idioma. También el auge de la
inteligencia artificial en latino américa, a crecido y son más la startups y
empresas que usan diferentes modelos, para su Core de negocio.

Los LLMs no están hechos para hablar español y es trabajo
del equipo de ciencia de datos, crear datasets y modelos que ayuden al idioma
español.

Esto lo puedes hacer de diferente manera, uno forma es la
construcción de datasets open source, haciendo scrapy de diferentes páginas de
internet, buscando disminuir los sesgos. Otra manera es la traducción de datasets
del inglés, por ejemplo, para entrenar stable difusión o IDEFCS, es bueno usar
ya datasets creados, traducirlos con algún modelo ya hecho y lo más importante,
revisar la traducción.

Si estás empezando un proyecto de NLP, te recomiendo adoptar
el active learning o una variación que me ha funcionado en mis diferentes
proyectos. Active learning es como el juego del huevo y la gallina, creas un
dataset pequeño etiquétalo y entrenas un modelo, el modelo lo vas a utilizar
para etiquetar más datos, vuelves a etiquetar una porción de datos donde la
incertidumbre del modelo es alta y vuelves a entrenar y así sucesivamente. También
puedes usar algún modelo ya pre-entrando, aquí la idea es muy similar, siempre
guardas el input y el output del modelo, y con tus habilidades de científico de
datos buscas donde están los limites o los sesgos del modelo.

Por ejemplo, si el objetivo es la trascripción de audio a
texto de citas médicas en Colombia, lo primero que haría es usar whisper o algún
servicio en la nube como Amazon trascribe, es probable que estos modelos no
estén diseñados a la terminología médica y fallé cuando trascriba el nombre de
algún medicamento o enfermedad, también es probable que con los diferentes acentos
que hay en Colombia, se muy complejo para el modelo y no detecté ciertas
palabras. Por ejemplo, whisper fue entrenado como muchos audios de España y
pocos de Latinoamérica.  Es por eso por
lo que ya tenemos en producción un modelo recolectando datos, el trabajo de los
científicos de datos, es identificar aquellos textos que no se transcribieron bien,
tener en mente los sesgos del modelo y así poder tener una versión 2.0, propia y
especializada al problema. Este va a seguir siendo el ciclo de vida del modelo,
recuerda que el siguiente problema es data drift, por ejemplo, que salga un nuevo
medicamento al mercado o una terapia, que el modelo nunca haya escuchado y este
afecta su funcionamiento.

Son muchos los retos hay como científicos de datos e ingeniero
de aprendizaje automático, recuerda siempre pensar en los sesgos de los datos.
