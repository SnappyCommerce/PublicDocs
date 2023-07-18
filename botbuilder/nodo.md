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



