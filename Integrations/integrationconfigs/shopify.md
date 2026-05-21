# Shopify

## Variables

### `ignoreUnavailableProducts`
**Tipo:** booleano (true / false)

Si se activa, se ocultan los productos que no tengan ninguna variante disponible para la venta.

---

### `ignoreCategoriesWithoutProducts`
**Tipo:** booleano (true / false)

Si se activa, se ocultan las colecciones/categorías que no tengan productos asignados.

---

### `providers`
**Tipo:** configuración especial

Permite asociar transportistas de Shopify a otras integraciones de seguimiento. Cuando la URL de tracking de un pedido contiene la clave configurada, el sistema consulta el estado a esa integración en lugar de usar el estado propio de Shopify.

> **Ejemplo:** si la clave es `andreani` y la URL de tracking es `https://tracking.andreani.com/...`, se delegará la consulta a la integración de Andreani.
