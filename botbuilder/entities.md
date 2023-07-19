# ¿Qué son las entities?

Las entities (entidades) son fragmentos o componentes específicos de un texto que tienen cierto significado. Representan información relevante dentro de una oración o un conjunto de palabras.

Son piezas clave de información dentro del input del usuario y pueden ser de dos tipos:

1) Entidades estáticas (Static): Estas entidades son predefinidas y su información se almacena en el bot, generalmente a través de sinónimos o patrones. Por ejemplo, si el bot maneja una tienda de ropa, una entidad estática podría ser "talla" con valores como "S", "M", "L”, etc
2)Entidades dinámicas (Dynamic): Estas entidades son simbólicas y no se pueden cargar con ejemplos o patrones. Son piezas de información que se capturan en tiempo real a través del input del usuario y se almacenan en el contexto de la conversación. Por ejemplo, una entidad dinámica podría ser el "nombre" del usuario o una fecha específica.

Esto nos permite tener dos comportamientos a la hora de agregar entidades en las intenciones.

Cuando agregamos una entity estático a un intent, el valor siempre deberá coincidir con los ejemplos del entity. En cambio, las entities dinámicas podrán tomar cualquier valor.

Es importante tener en cuenta que las entidades estáticas pueden tener sinónimos o patrones predefinidos para que el bot pueda reconocerlas de manera más efectiva. En cambio, las entidades dinámicas se capturan en tiempo real y se almacenan en el contexto de la conversación.

## Entidades en el código

Una entidad puede ser un objeto (qué es un objeto? convendría definirlo antes?) o indefinido (un qué indefinido?) dependiendo de si se encuentra o no en el input del usuario. Una entity será undefined si es que no se encuentra en el input del usuario, de lo contrario será un objeto. Si la entidad existe en el input del usuario, entonces será un objeto con propiedades como "value", "literal" y "option" (explicar la propiedades).

## Comportamiento

Como mencionamos previamente, las entidades son objetos pero dependiendo el contexto su valor se puede transformar en la propiedad value. En el contexto, se guardará la propiedad de value y no el objeto. De todos modos se puede aclarar qué propiedad se desea guardar en el contexto. 
