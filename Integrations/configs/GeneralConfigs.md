# Configuraciones generales de integraciones

## Mapeos de estado condicionales

Normalmente, Snappy traduce el estado crudo de una plataforma usando una tabla fija. Pero a veces una tienda necesita que el mismo estado se traduzca diferente según otras propiedades del pedido. Para eso existe este mecanismo.

**Dónde se configura:** Panel de Snappy → Integraciones → [integración] → Campos personalizados → campo `orderStepsStatuses` con tipo `json`.

---

### Cómo funciona

Cada regla condicional tiene tres partes:

| Campo | Qué es |
|---|---|
| `dataType` | Debe ser `json` para que el condicional funcione |
| `integrationValue` | Un JSON con las condiciones que debe cumplir el pedido |
| `snappyValue` | El estado Snappy a devolver si se cumple la condición |

Cuando llega un pedido, Snappy revisa **todas las reglas en orden**. Para cada una verifica si el pedido contiene **exactamente** las propiedades indicadas en `integrationValue`. En cuanto encuentra la primera que cumple, devuelve ese `snappyValue` y **no sigue revisando** las demás. Solo si ninguna regla aplica, usa la tabla de mapeo estándar.

> [!IMPORTANT]
> Las condiciones son inclusivas — el pedido puede tener más campos de los que indica la condición. Snappy solo verifica que los campos especificados existan con esos valores exactos.

---

### Valores válidos

**`integrationValue`** — debe ser un JSON válido que describa propiedades del objeto de pedido crudo de la plataforma. Se puede usar cualquier campo que devuelva la API, incluyendo campos anidados:

```json
{ "status": "shipped" }
```
```json
{ "shipping_status": "shipped", "payment_status": "pending" }
```
```json
{ "shipping_option": { "service_code": "pickup_store" } }
```

**`snappyValue`** — debe ser uno de los códigos de estado válidos de Snappy (ver tabla de referencia en el doc principal): `created`, `shipped`, `delivered`, `canceled`, `awaiting-payment`, etc.

---

### Ejemplo completo

**Situación:** Una tienda tiene pedidos que aparecen como `shipped` en la plataforma, pero cuando el pago está pendiente no deberían mostrarse como enviados. La tienda quiere que en ese caso Snappy muestre `awaiting-payment`.

**Regla configurada:**
```json
integrationValue: {"shipping_status": "shipped", "payment_status": "pending"}
snappyValue: "awaiting-payment"
```

**Pedido que llega de la plataforma:**
```json
{
  "id": 98765,
  "status": "open",
  "payment_status": "pending",
  "shipping_status": "shipped",
  "total": "4500.00"
}
```

**Resultado:** Snappy comprueba si el pedido contiene `shipping_status: "shipped"` ✅ y `payment_status: "pending"` ✅ — ambas condiciones se cumplen, por lo que devuelve `awaiting-payment` **sin llegar al mapeo estándar**.

---

**Mismo pedido, pero con pago aprobado:**
```json
{
  "payment_status": "paid",
  "shipping_status": "shipped"
}
```

**Resultado:** La condición no aplica porque `payment_status` es `"paid"`, no `"pending"`. Snappy cae al mapeo estándar y devuelve `shipped`.

---

### Comportamiento con múltiples reglas

Si hay más de una regla configurada, Snappy las evalúa **en el orden en que están guardadas** y usa la primera que aplique:

```
Regla 1: {"status": "shipped", "service_code": "pickup"} → "awaiting-pickup"
Regla 2: {"status": "shipped"}                           → "shipped"
```

Si el pedido tiene `status: "shipped"` y `service_code: "pickup"` → aplica la Regla 1 (`awaiting-pickup`).  
Si el pedido tiene solo `status: "shipped"` → no aplica la Regla 1, aplica la Regla 2 (`shipped`).

> ⚠️ El orden importa. Una regla más específica (más condiciones) debería ir antes que una más general, de lo contrario la general la pisaría primero.
