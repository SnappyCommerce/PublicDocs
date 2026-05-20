# Google Sheets

## Variables

### `url`
**Tipo:** texto

URL del Google Sheet que contiene los datos de **pedidos** (para consulta de estado de órdenes).

---

### `sheet`
**Tipo:** selector

Hoja del spreadsheet de pedidos donde están los datos. Se selecciona de una lista que carga automáticamente una vez ingresada la URL — no requiere completar manualmente.

---

### `productUrl`
**Tipo:** texto

URL del Google Sheet que contiene los datos de **productos**. Puede ser el mismo spreadsheet que `url` o uno distinto.

---

### `productSheet`
**Tipo:** selector

Hoja del spreadsheet de productos que contiene los datos principales de cada producto. Se selecciona de una lista que carga automáticamente — no requiere completar manualmente.

---

### `variantsSheet`
**Tipo:** selector

Hoja del spreadsheet de productos que contiene los datos de variantes. Se selecciona de una lista que carga automáticamente. Solo se configura si las variantes están en una hoja separada — no requiere completar manualmente.
