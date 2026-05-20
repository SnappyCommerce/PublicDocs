# VTEX

## Variables

### `masterDataVersion`
**Tipo:** opciones (`V1` / `V2`)

Versión del Master Data de VTEX que usa la tienda. Determina cómo se consultan los datos de clientes. Confirmar con el equipo técnico del cliente cuál versión tienen activa.

---

### `sellerId`
**Tipo:** texto

ID del vendedor en VTEX. Se usa al generar el carrito de compra. Para tiendas que venden directamente (sin marketplace), el valor es `1`.

> **Valor por defecto:** `1`

---

### `showDetailedDescription`
**Tipo:** booleano (true / false)

Si se activa, agrega el campo "Composición y Cuidado" a la descripción del producto. Útil para tiendas de indumentaria que tienen ese atributo cargado en VTEX.

---

### `filterByStock`
**Tipo:** booleano (true / false)

Si se activa, solo se muestran productos que tengan al menos una variante con stock disponible.

---

### `ignoreCategoriesWithoutSubcategories`
**Tipo:** booleano (true / false)

Si se activa, se ocultan las categorías que no tengan subcategorías. Útil para evitar mostrar categorías vacías o de navegación en el árbol de categorías.

---

### `additionalProductFilters`
**Tipo:** tabla de clave/valor

Filtros permanentes que se aplican a todas las búsquedas de productos. Solo se mostrarán productos que cumplan con todos los filtros configurados aquí, independientemente de lo que busque el usuario.

Los valores válidos de clave y valor dependen de los atributos configurados en el catálogo de VTEX del cliente — consultar con su equipo técnico.

> **Ejemplo:** clave `B` (marca en VTEX), valor `Nike` — el bot solo mostrará productos de la marca Nike en cualquier búsqueda.

---

### `maxCategoryDepth`
**Tipo:** número entero

Cantidad máxima de niveles del árbol de categorías que se importan desde VTEX. A mayor número, más subcategorías se incluyen.

> **Valor por defecto:** `10`

---

### `providers`
**Tipo:** configuración especial

Permite asociar transportistas de VTEX a otras integraciones de seguimiento. Cuando un pedido tiene un courier que coincide con uno de los configurados, el sistema consulta el estado a esa integración en lugar de usar el estado propio de VTEX.

> **Ejemplo:** si el courier es `andreani`, se puede configurar para que la consulta de estado se delegue a la integración de Andreani.
