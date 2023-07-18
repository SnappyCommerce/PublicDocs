# ¿Qué es un Package?

Un Package (Paquete) es un tipo de estructura o brain que puede ser importado a otras estructuras.

Es importante tener en cuenta que cuando se utiliza un paquete, existe la posibilidad de que el bot no encuentre un nodo para procesar y quede atrapado infinitamente dentro del paquete. Por esta razón, es importante diseñar los paquetes de manera que exista una salida definida. Una forma de proporcionar una salida clara es utilizando los nodos Exit Package, que se desarrollarán más adelante.

Al insertar un paquete a un brain, este se entrenará con todas las intenciones (intents) que contenga el paquete. Las intenciones de un paquete no deben repetirse respecto a otros paquetes ni al brain principal.

Si el paquete contiene entities (entidades) preexistentes en la estructura principal, no habrá conflictos, ya que cada paquete y cada brain sólo tendrá acceso a sus propias entidades.

Dentro de los paquetes, se pueden configurar Parameters (parámetros). Los parámetros son configuraciones que sirven para enviar información del brain principal a un paquete. Los parámetros se almacenan dentro de la sección de Contextos, como $parameters.

Es importante tener en cuenta que los parámetros no se pueden modificar durante la ejecución del bot, ya que están diseñados para proporcionar información adicional o configuraciones específicas que se necesitan en un momento determinado.

Esto significa que, una vez le enviamos un parámetro a un paquete, dicho dato permanecerá igual hasta que salgamos del paquete. Para modificar el parámetro, es necesario que el flujo vuelva a ingresar al paquete deseado.

# ¿Cómo crear un Package?

1) Dentro del Bot Builder, clickear sobre el botón Create Package (Crear paquete)
2) Asignarle un nombre al paquete (máximo 22 caracteres). El nombre no puede finalizar con un espacio o el sistema lo tomará como invalido.
3) Agregar los elementos que se deseen para el paquete en cuestión: nodos, contextos, etc. (los elementos se desarrollarán en las secciones siguientes).

![package](/images/botbuilder/package/nodoRespuesta.gif)

# ¿Cómo publicar un Package?

Para que se pueda utilizar un Package es necesario publicarlo, es decir hacer un deploy a la versión deseada. Al hacer deploy, el paquete queda disponible para utilizar en un brain.

☝️Para hacer deploy, el paquete debe contener al menos un nodo. Esto aplica también a la estructura principal.

Una vez que el package está publicado, se puede acceder a él desde un brain a través de un nodo específico, un nodo Package (nodo Paquete).

En principio, solo se podrá acceder a un paquete desde el brain de la tienda a la que ambos pertenecen. Para que un paquete esté accesible a otras tiendas, se lo debe configurar como Public Package (Paquete público), desde el apartado Settings (Configuración).

