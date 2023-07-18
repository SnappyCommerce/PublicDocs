# ¿Qué es un nodo?

Un nodo es un elemento básico que forma parte de la estructura conversacional del bot. Los nodos permiten diagramar la estructura lógica del bot, para que éste pueda manejar las interacciones con los usuarios. 

Cada nodo representará una respuesta potencial a una entrada (input) del usuario. A cada nodo se le debe asignar un nombre y asociarle una condición.

El brain evaluará las condiciones de los nodos uno por uno, de manera secuencial. Si la condición en un nodo es verdadera, este se ejecutará.

La estructura conversacional puede incluir nodos principales (main nodes) y nodos hijos (child nodes). Los nodos hijos son aquellos que dependen de un nodo principal. A continuación se muestra un ejemplo, con dos nodos principales y un nodo hijo para cada uno:

imagen

Existen distintos tipos de nodos: Estandard, Carpetas, Package (Paquete) y Exit (Salida).
