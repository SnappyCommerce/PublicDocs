# ¿Qué son los Intents?

Los Intents (intenciones) corresponden al propósito o la acción que un usuario quiere llevar a cabo con su entrada de texto. Representan la intención subyacente detrás de la solicitud del usuario. Los Intents (intenciones) se refieren a las posibles acciones o respuestas que el bot puede tomar en función del input del usuario. 

Por ejemplo: Se agrega una intención para la búsqueda de productos y se entrena con el ejemplo "Quiero ver sus productos". Cuando el bot reconozca esa intención, se podrá dirigir al usuario a ver los productos.

Es importante que los intents tengan la mayor cantidad de ejemplos posibles de calidad, para que el bot pueda reconocer correctamente los patrones y tomar la acción correcta en respuesta a la entrada del usuario.

Los intents también pueden incluir entities (entidades), es decir, piezas de información en sus ejemplos (Ver ¿Qué son las entities?)

# ¿Cómo agregar intents? (paso a paso)

- Ir al menú lateral > Intents
- Clickear en Add
- Agregar Name (nombre)
- Incluir a qué hace referencia la intención (este campo no es obligatorio).
- Agregar los ejemplos como se muestra en la imagen (más ejemplos es mejor mientras sean de calidad!)

![intents](/images/botbuilder/intents/intents.png)

## ¿Cómo incluir intents con entities?

(Antes de comenzar, recomendamos ver la sección ¿Qué son las entities?)

Para agregar una entidad a un ejemplo, se utiliza la sintaxis "@nombreDeEntidad". Veamos un caso, si se tiene una entidad llamada "nombre", se podría crear una intención que reconozca el nombre del usuario en un ejemplo como: "Hola, yo soy @nombre".

De esta manera, cuando el usuario ingresa un input similar al ejemplo, el bot reconocerá la entidad "nombre" y podrá utilizar esa información para proporcionar una respuesta más personalizada y relevante.

## Intents fuera del scope

Un intent fuera del scope se refiere a una situación en la que el bot reconoce una intención que no está dentro del ámbito actual de la conversación.

Si el procesamiento de la conversación está dentro de un paquete, el ámbito se limita al paquete. En este caso, solo se reconocerán y responderán a los intents dentro del paquete actual. Si el bot reconoce un intent fuera de este ámbito, aparecerá como "#other"

Por ejemplo: Un bot está diseñado para reconocer y responder a preguntas sobre el clima, pero el usuario pregunta sobre el tráfico. El bot puede reconocer la intención como "fuera de scope" o "no reconocida". En este caso, el bot podría responder con un mensaje como "Lo siento, no puedo ayudarte con esa pregunta".

Es importante diseñar y entrenar el bot con suficientes ejemplos para reconocer correctamente los intents y manejar los casos fuera de scope para proporcionar una experiencia de conversación efectiva y satisfactoria para el usuario.


## Intents reconocidos

La variable "intents" en el código del bot es un arreglo de objetos que contiene información sobre los intents reconocidos válidos para el ámbito actual de la conversación. Cada objeto dentro del arreglo tiene dos propiedades:

1) "intent": Es una cadena de texto que representa el nombre del intent reconocido por el bot.
2) "score": Es un número que indica la confianza del bot en que el intent reconocido es el correcto.

Es importante utilizar esta información para seleccionar la mejor respuesta o acción para el usuario, basándose en el intent reconocido y su confianza asociada.


