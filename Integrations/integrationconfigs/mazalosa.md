# Mazalosa

## Variables

### `prefixes`
**Tipo:** texto (valores separados por coma)

Lista blanca de prefijos: solo se consultan los pedidos cuyo número de orden empiece con alguno de los prefijos indicados. Los pedidos que no coincidan son ignorados.

> **Ejemplo:** `MAZ-` filtra solo los pedidos cuyo número empieza con `MAZ-`. Para múltiples prefijos: `MAZ-,ORD-`.

---

### `brands`
**Tipo:** texto (valores separados por coma)

Lista blanca de marcas: solo se consultan los pedidos que pertenezcan a alguna de las marcas indicadas. Los pedidos de otras marcas son ignorados.

---

### `fpro`
**Tipo:** texto

Código de perfil utilizado para consultar el saldo de puntos de fidelidad de los clientes en Mazalosa. Lo provee el equipo de Mazalosa al momento de la integración.
