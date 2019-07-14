# Dialogflow Flask

### Pre requisitos

* Python 3.6+
* DialogFlow
* VirtualEnv
* Ngrok

### Estructura del proyecto

```
Raiz del proyecto
│───README.md
│───.env
│───.flaskenv
│───requirements.txt
│───index.py
└───static
│   │───index.js
│   │───main.css
│   └───images
│       │───...
└───templates
│    │───index.html
```

### Creando un entorno virtual

Virtualenv es una herramienta para crear entornos virtuales para python. Esto representa una ventaja con respecto a si trabajamos varios proyectos a la vez y cada uno tiene distintas dependencias, por lo tanto cada entorno virtual que creemos tendrá sus librerias correspondientes y no creará conflicto con las demás.

Para crear el entorno virtual se utiliza el siguiente comando en la raiz del proyecto:

`virtualenv --python=python3 venv`

Luego hay que activar el entorno virtual en la raiz del proyecto:

`source venv/bin/activate`

Si queremos desactivar el entorno virtual:

`deactivate`

### Instalando dependencias

Para instalar las dependencias que utiliza este proyecto debemos correr el siguiente comando en la raiz del proyecto:

`pip install -r requirements.txt`

### Agregando configuración de entorno para Flask

Dentro del archivo .flaskenv, crearlo si no existe, debemos agregar las siguientes lineas:

`FLASK_APP=index.py`
`FLASK_ENV=development`

Con esto le estamos aclarando a Flask que el archivo main de la app es "index.py" y que se ejecute en un entorno de desarrollo.

### DialogFlow

DialogFlow es la herramienta de creación de chatbots capaz de entender el lenguaje natural que Google pone a disposición de todos aquellos que quieran iniciarse en el desarrollo de estas tecnologías conversacionales.

#### Creando nuestro agente

Es necesario entrar a [DialogFlow](https://console.dialogflow.com) con una cuenta de Google, por lo general todos tenemos una cuenta ya creada de google.

![](https://images.ctfassets.net/1es3ne0caaid/5GQPRDCKtyOi66s80Y8U68/70f0146c00c66c601eb8fa97ef71c4f9/flask-movie-chatbot-create-agent-1.png)

* Hacer click en **create agent**
* Ahora hay que ponerle algún nombre a nuestro agente y luego hacer click en **create**

![](https://images.ctfassets.net/1es3ne0caaid/2CYazV0QZu6CqYkOACYo2Y/d49642e802411ea41ac098ca5aa02a03/flask-movie-chatbot-create-agent-2.png)

#### Intents

Los intents o intenciones en DialogFlow definen un comportamiento específico de la conversación, en los intents debemos completar las frases de entrenamiento y las respuestas por default. También podemos agregar a un intent acciones y parametros por ejemplo para la frase de entrenamiento "Mi nombre es leo" se puede asignar un parametro a "leo" especificando que es un nombre y además se puede asignar una pregunta al parametro, como lo puede ser "¿cuál es tu nombre?" por lo tanto el bot podría preguntar cual es mi nombre, yo responderia y el bot guardaria mi nombre en el parametro correspondiente.

![](https://images.ctfassets.net/1es3ne0caaid/6NeWf8bKZqQiq4goqCU8U2/d1bc6d6b04d8709888021ea2b8d12f08/flask-movie-chatbot-create-intent.png)

Si queremos agregar respuestas personalizadas que pasen por nuestra API en Flask debemos habilitar la siguiente opción:

![](https://images.ctfassets.net/1es3ne0caaid/7lWUP3qTVSeisQyOyyya0S/a0efe1e41828bba1d533f86e97542d82/flask-movie-chatbot-enable-webhooks-dialogflow.png)

### Obteniendo nuestra API KEY

Para que Flask se pueda conectar a nuestro proyecto de DialogFlow debemos ingresar a [Google Cloud Platform](http://console.cloud.google.com)

![](https://images.ctfassets.net/1es3ne0caaid/2zayoVzgYImGGC4uSQuO2u/fb2198e11835999fc34c0344337a94b4/flask-movie-chatbot-project-id.png)

* Elegimos el proyecto relacionado al nuestro creado en DialogFlow
* Copiamos el project_id dentro del archivo .env

`DIALOGFLOW_PROJECT_ID=project_id`

* Ahora debemos ir a *Apis y Services* y luego seleccionamos *Credentials*

![](https://images.ctfassets.net/1es3ne0caaid/1xuMA0MTLacI6cSWO8akae/3614a95f9d637a4579a495f15f7e9b87/flask-movie-chatbot-credentials-1.png)

* Seleccionamos *create credentials* y luego *service account key*

![](https://images.ctfassets.net/1es3ne0caaid/50gwnohvMc4SIosIs8ai4Y/b1a99b0f1a314b340db153e117f8e5d2/flask-movie-chatbot-credentials-2.png)

* Seleccionamos *DialogFlow Integrations* y elegimos que la key sea de tipo *json*

![](https://images.ctfassets.net/1es3ne0caaid/2rA9saLAxe8WmyAguEIoQG/a23b2d638857f5f226da4e4b94790e8a/flask-movie-chatbot-credentials-3.png)

* Le damos al botón *create*, esto nos descargara un archivo .json que debemos mover a la raiz de nuestro proyecto
* Para terminar debemos agregar el path del archivo a nuestro archivo .env

`GOOGLE_APPLICATION_CREDENTIALS=tuproyectoengoogle-*.json`

### Creando nuestro webhook

Anteriormente describi que si queriamos respuestas personalizadas teniamos que activar la opción de fullfillment en nuestro intent, además de eso tenemos que crear un endpoint especifico de nuestra API en Flask para que la respuesta al cliente la podamos crear.

![](https://i.ibb.co/HKjwWdM/Captura-de-Pantalla-2019-07-13-a-la-s-21-54-49.png)

Actualmente en el código el webhook se encuentra de la manera anterior para que pueda ser modificado como corresponda y a las necesidades de cada uno.

### Exponiendo nuestro localhost

DialogFlow no funciona con localhost por lo tanto tenemos que exponerlo a la internet, una manera simple es con [ngrok](https://ngrok.com/). Lo que realiza ngrok es crear un tunel desde nuestra pc a una IP que nos dan ellos para que podamos usar, la realidad es que para pruebas como estas ngrok cumple bastante.

Por lo tanto para exponer nuestro localhost debemos correr el siguiente comando:

`ngrok http localhost:5000`

Donde 5000 es el puerto por default de nuestra aplicacion en Flask, si seteaste otro puerto en flask debes cambiarlo en el comando.

Ngrok nos mostrara algo como esto:

![](https://images.ctfassets.net/1es3ne0caaid/vXOBTKNbvqUwwOGmismqi/125be19ef53303b7cca6a321b30f77a5/flask-movie-chatbot-ngrok.png)

Debemos copiar el enlace https que se verá parecido al siguiente: https://0a36e90a.ngrok.io

Ahora debemos:

* Ir a la consola de DialogFlow y clickear en la opción **FullFillment*
* Habilitar la opción de **webhook**
* En el campo **URL** debemos pegar la url que copiamos anteriormente, agregandole el endpoint que creamos para el webhook. Tendria que quedar algo parecido a esto https://0a36e90a.ngrok.io/webhook
* Hacer click en el botón **save**

![](https://images.ctfassets.net/1es3ne0caaid/50cxD3eVjyWUSsW2uqEE2e/75ebb596a377551552600932adddc150/flask-movie-chatbot-add-ngrok-to-dialogflow.png)

### Probando

Ahora hay que levantar Flask con:

`flask run`

Deberias poder ingresar al link [http://localhost:5000](http://localhost:5000) y ver:

![](https://i.ibb.co/wYR6ksF/Captura-de-Pantalla-2019-07-13-a-la-s-22-28-41.png)
![](https://i.ibb.co/5TKd5hQ/Captura-de-Pantalla-2019-07-13-a-la-s-22-28-51.png)

Fuente: [https://pusher.com/tutorials/chatbot-flask-dialogflow](https://pusher.com/tutorials/chatbot-flask-dialogflow)
