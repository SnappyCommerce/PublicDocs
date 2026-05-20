# Tienda Nube

## Variables

### `abandonedCartHours`
**Tipo:** número entero

Cantidad de horas que deben pasar desde que un cliente abandona un carrito para que sea considerado "abandonado". Si se deja en `0`, la funcionalidad de carritos abandonados queda deshabilitada.

---

### `providers`
**Tipo:** configuración especial

Permite asociar transportistas de Tienda Nube a otras integraciones de seguimiento. Cuando un pedido tiene un courier que coincide con uno de los configurados, el sistema consulta el estado a esa integración en lugar de usar el estado propio de Tienda Nube.

> **Ejemplo:** si el courier es `andreani`, se puede configurar para que la consulta de estado se delegue a la integración de Andreani.

---

### `shippingReferenceMessages`
**Tipo:** tabla de clave/valor

Mensajes personalizados que se muestran según la referencia de la opción de envío del pedido. Cuando un pedido tiene una referencia de envío que coincide con una clave configurada, se agrega ese mensaje a la respuesta del bot.

> **Ejemplo:** clave `retiro_local`, valor `Podés retirar tu pedido en nuestro local de lunes a viernes de 9 a 18 hs.`
