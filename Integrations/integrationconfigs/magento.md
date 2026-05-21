# Magento (2.3)

## Variables

### `imagePath`
**Tipo:** texto

Ruta dentro del dominio de la tienda donde están alojadas las imágenes de los productos. Solo se configura si Magento usa una ruta no estándar.

> **Valor por defecto:** `media/catalog/product`

---

### `storeView`
**Tipo:** texto

ID de la vista de tienda en Magento. Se usa para filtrar la **búsqueda de productos** por tienda, en instalaciones con múltiples vistas (por ejemplo, por idioma o país). Si la tienda tiene una sola vista, se puede dejar vacío.

---

### `storeViewId`
**Tipo:** texto

ID de la vista de tienda en Magento. Se usa para filtrar los **carritos abandonados** por tienda. 

---

### `abandonedCartHours`
**Tipo:** número entero

Cantidad de horas que deben pasar desde que un cliente abandona un carrito para que sea considerado "abandonado". Si se deja en `0`, la funcionalidad de carritos abandonados queda deshabilitada.

---

### `filterByStock`
**Tipo:** booleano (true / false)

Si se activa, solo se muestran productos que tengan el flag `is_in_stock` en `true` en Magento.

---

### `filterByStockQty`
**Tipo:** booleano (true / false)

Si se activa, solo se muestran productos con cantidad de stock mayor a `0`. Se puede usar junto con `filterByStock` — en ese caso se aplican ambos filtros.

---

### `hidePrice`
**Tipo:** booleano (true / false)

Si se activa, no se muestra el precio de los productos en las respuestas del bot.

---

### `searchByCategoryField`
**Tipo:** booleano (true / false)

Cambia cómo se obtienen los productos al navegar por categorías. No todas las tiendas Magento son compatibles con ambos métodos.

> Dejar en `false` por defecto. Activar solo si el equipo técnico del cliente lo indica.

---

### `productChildVisibility`
**Tipo:** texto (valores separados por coma)

Lista de códigos de visibilidad de Magento que determinan qué variantes de un producto configurable se muestran. En Magento, los códigos de visibilidad son: `1` = no visible individualmente, `2` = catálogo, `3` = búsqueda, `4` = catálogo y búsqueda.

> **Valor por defecto:** `1`
>
> **Ejemplo:** `1,2` muestra variantes con visibilidad "no visible individualmente" y "catálogo".

---

### `fillOrderIdSize`
**Tipo:** número entero

Si los IDs de pedido en Magento tienen un tamaño fijo (por ejemplo, siempre 8 dígitos), este valor completa con ceros a la izquierda hasta alcanzar ese tamaño.

> **Ejemplo:** con valor `8`, el pedido `123` se busca como `00000123`.

---

### `providers`
**Tipo:** configuración especial

Permite asociar métodos de envío de Magento a otras integraciones de seguimiento. Cuando un pedido tiene un método de envío que coincide con uno de los configurados, el sistema consulta el estado del pedido a esa integración (usando el tracking code de Magento) en lugar de usar el estado propio de Magento.

> **Ejemplo:** si el método de envío es `andreani_standard`, se puede configurar para que la consulta de estado se delegue a la integración de Andreani.
