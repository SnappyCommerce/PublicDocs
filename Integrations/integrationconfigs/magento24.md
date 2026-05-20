# Magento (2.4)

## Variables

### `productBaseUrl`
**Tipo:** texto

URL base que se usa para generar los links de los productos. Solo se configura si el sitio frontend está en un dominio distinto al de la API de Magento. Si se deja vacío, se usa la misma URL de la integración.

---

### `storeView`
**Tipo:** texto

Código de la vista de tienda en Magento. Se envía en cada consulta para obtener los productos del idioma o región correspondiente. Si la tienda tiene una sola vista, se puede dejar vacío.

---

### `facets`
**Tipo:** texto (pares `clave:nombre` separados por coma)

Lista de atributos de Magento que se pueden usar como filtros en la búsqueda de productos. 

> **Ejemplo:** `color:Color,size:Talle`
