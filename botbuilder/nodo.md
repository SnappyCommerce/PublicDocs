# ¿Qué es un nodo?

Un nodo es un elemento básico que forma parte de la estructura conversacional del bot. Los nodos permiten diagramar la estructura lógica del bot, para que éste pueda manejar las interacciones con los usuarios. 

Cada nodo representará una respuesta potencial a una entrada (input) del usuario. A cada nodo se le debe asignar un nombre y asociarle una condición.

El brain evaluará las condiciones de los nodos uno por uno, de manera secuencial. Si la condición en un nodo es verdadera, este se ejecutará.

La estructura conversacional puede incluir nodos principales (main nodes) y nodos hijos (child nodes). Los nodos hijos son aquellos que dependen de un nodo principal. A continuación se muestra un ejemplo, con dos nodos principales y un nodo hijo para cada uno:

imagen

Existen distintos tipos de nodos: Estandard, Carpetas, Package (Paquete) y Exit (Salida).

## Nodo Estandard

El nodo estandard al ser procesado puede ejecutar diferentes acciones:

 → Response (Respuesta): 

En esta opción se definirán la/s respuesta/s de texto que devolverá el nodo al ser procesado. Las respuestas se pueden incluir en formato de texto plano o en formato de texto JSON. El output tendrá las respuestas en el orden preestablecido. 

NOTA: La respuesta se ejecuta después de las acciones y antes de los contextos.

![nodo](/images/botbuilder/nodo/nodoRespuesta.gif)

 → Jumps (Saltos):

Esta función permite “saltar” hacia un nodo específico. Existen 3 tipos distintos de saltos:

1) Respond: Procesa directamente el nodo especificado. No toma en cuenta la condición, por lo tanto, no importará si es true (verdadera) o no.
2) Wait for user input: Este esperará al input del usuario. Luego de este input comenzará a evaluar el nodo especificado.
3) Respond and evaluate: Esta acción permite que, luego de ejecutarse un nodo, este salte a otro según se seleccione previamente. A partir del nodo seleccionado, se evaluarán los nodos en orden consecutivo hasta ejecutar el primer nodo que sea verdadero.

Jump back (Volver a saltar): Los saltos cuentan con una función llamada Jump Back o Volver a saltar. ¿Cómo funciona?
- Se procesa un nodo que incluye un jump back.
- Luego de realizar el tipo de salto indicado, se vuelve a evaluar el nodo con jump back.
- El jump back indica evaluar los nodos siguientes al nodo que lo posee (sean o no nodos hijos de éste).
- Se evaluarán los nodos en el orden preestablecido hasta que se cumpla la condición de uno de ellos.
- El Jump back se ejecuta una sola vez. Luego de visitar un segundo nodo de forma exitosa, se requerirán nuevos saltos para continuar con el flujo conversacional.

→ Context (Contexto):

Los contextos son campos variables de la estructura que se pueden usar en cada nodo. Sirven para almacenar distintos tipos de datos: string (alfanuméricos), number (numéricos), boolean (verdadero o falso), object (valores compuestos), array (arreglo, o bien, listas del mismo tipo de datos).

Los contextos se pueden modificar desde el apartado Context de cada nodo. 

Supongamos un ejemplo. Queremos definir un contexto en el que el usuario guarde su nombre. Los pasos a seguir son:

- Ir al apartado Context (contextos) y definir el contexto "nombreDeUsuario" como string y dejar vacío el campo Value o completar con un valor por defecto (Por ejemplo: Se podría completar con la palabra “usuario” para que en caso de no recibir el nombre, quede con el valor por defecto).
- Luego, creamos un nodo que solicite un nombre al usuario cuya condición sea verdadera. Para esto, en el campo Text Response ingresamos: "Por favor, ingrese su nombre".
- Agregaremos un nodo hijo del nodo principal (recordá que su condición sea verdadera).
- En el nodo hijo, vamos al apartado de contextos y escribimos el contexto ya definido "nombreDeUsuario" en el campo Key.
- En el campo siguiente, Value, agregaremos “input.text”. De esta forma, se tomará como valor, la entrada del usuario.
- Para el campo Type, seleccionaremos la opción Code (Esto es muy importante, si quedara como String, se tomaría como valor la frase input.text).

![context](/images/botbuilder/Context/Context.png)

ACLARACIÓN: Normalmente, si vamos a usar una sola línea de código a la que no se agregará nada, es conveniente usar el tipo de contexto "code".

- Actions (Acciones): 

Las acciones permiten hacer solicitudes del tipo HTTP.
Podemos usar solicitudes de funciones ya hechas, nosotros por nuestro lado deberemos aportar los parámetros que solicitan: URL, METHOD, HEADERS y BODY.

URL: Este parámetro corresponde al enlace al cuál haremos la solicitud. Por ejemplo: www.ejemplo.com/ejemplo-123 

METHOD: Este parámetro es el método en que se realizará la solicitud y depende de cómo esté configurada la URL en cuestión. El método puede ser de tipo:

- GET: para obtener información.
- POST: para enviar información para su procesamiento.
- PUT: para actualizar información. 
- DELETE: para eliminar información.

HEADERS: Este parámetro corresponde al encabezado de la solicitud. Estos permiten enviar información adicional junto a una petición o respuesta. Este campo se debe completar en formato JSON.

BODY: Este parámetro es el cuerpo de la petición, refiere al contenido que recibirá la solicitud web. Este campo se debe completar en formato JSON, de acuerdo al contenido que se desee incluir en la solicitud.

Las acciones son lo primero que se ejecuta cuando un nodo es procesado. Es decir, se ejecutan antes que las respuestas, saltos y/o contextos.

Se ejecutan en el orden establecido y los resultados se guardarán en un arreglo en el contexto predeterminado $actionResults. Por lo que, si deseamos acceder al resultado de la primera acción, podemos acceder mediante $actionResults.

- Info (Información):

En esta pestaña se encuentran tres datos del nodo correspondiente:

El UUID es el identificador único universal de cada nodo y se trata de un código alfanumérico.

Si se trata de un nodo hijo, el campo Parent (padre) indicará el código UUID del nodo principal del cual éste es hijo. Si se trata de un nodo principal, indicará: “Null” (Nulo) ya que no sería hijo de ningún nodo.

Si se trata de un nodo hijo, el campo PrevSibling (hermano previo) indicará el código UUID del nodo inmediatamente anterior que comparta el mismo nodo principal. Si se trata de un nodo principal, indicará: “Null” (Nulo) ya que no sería hermano de ningún nodo.

## **Carpetas**

  Las carpetas son nodos que por defecto tienen la condición en true. La carpeta se utiliza para agrupar nodos y no afecta el flujo principal del bot. Los nodos dentro de la carpeta solamente serán evaluados si la condición fue verdadera o si el flow ya estaba dentro de la carpeta.

## Nodo Package

Un nodo Package (nodo Paquete) se utiliza para conectar un paquete a un brain. Por defecto, los nodos de tipo Package tienen la condición en true, lo que significa que siempre se procesarán a menos que se especifique lo contrario. 

Cuando se entra en un paquete, el paquete toma el control del flujo de la aplicación y el bot comienza a procesar los nodos dentro del paquete. 

Para devolver el control al brain principal, es necesario que el paquete entre en un nodo de Exit Package o que, al evaluar, ninguno de los nodos dentro del paquete devuelva verdadero.

Si el paquete sale mediante un nodo Exit Package (Salir del paquete), se evaluarán instantáneamente los nodos hijos del paquete. Esto significa que el paquete dejará de controlar el flujo de la aplicación y el control volverá al brain principal.

Se puede acceder al contexto interno del package mediante **$internal.packages.UUID_DEL_NODO**.

Para más información de cómo utilizar los packages, visitar la sección ¿Qué es un Package?

### Context Mapping 

El Context mapping o Mapeo de contexto es una configuración que sirve para extraer datos del paquete hacia el brain principal.

La configuración context mapping en un nodo de tipo package sirve para mapear un contexto del paquete en cuestión a un contexto del brain. Es decir, el valor indicado del contexto del paquete se replicará en el contexto del brain que se le indique.

En términos simples, nos permite “sacar” o “pasar” un contexto de un paquete al brain.

## Exit Package 

Un nodo Exit Package (Salida del paquete) solo puede ser utilizado dentro de un paquete. Cuando su condición es verdadera, le indica al paquete que debe devolver el control a la estructura principal.

Al usar este nodo se evaluarán los nodos hijos del nodo de tipo package.

