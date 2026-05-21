# Shipnow

## Variables

### `hideEstimatedDate`
**Tipo:** booleano (true / false)

Si se activa, no se muestra la fecha estimada de entrega en las respuestas del bot.

---

### `regexWhitelist`
**Tipo:** lista de textos

Lista de patrones para filtrar qué números de seguimiento se consultan. Solo se procesan los pedidos cuyos números de tracking coincidan con al menos uno de los patrones ingresados.

> Si no se configura, se consultan todos los pedidos sin filtro.
>
> **Ejemplo:** `^SN` filtra solo los trackings que empiezan con `SN`.

---

### `maxAgeDays`
**Tipo:** número entero

Cantidad máxima de días de antigüedad que puede tener un pedido para ser consultado. Pedidos más antiguos que este valor no se incluyen en las respuestas.

> Si no se configura, no hay límite de antigüedad.
