# SAP

## Variables

### `accountId`
**Tipo:** texto

Identificador de la cuenta en SAP a la que se asocian los contactos creados. Lo provee el equipo técnico del cliente.

---

### `contactModel`
**Tipo:** opciones (`Contacts` / `individualCustomers`)

Determina qué modelo de datos de SAP se usa para crear y consultar contactos:

- **`Contacts`**: usa el modelo de personas de contacto (`contactPersons`). Requiere configurar `accountId`.
- **`individualCustomers`**: usa el modelo de clientes individuales (`individualCustomers`). Requiere configurar `customerRole`.

---

### `customerRole`
**Tipo:** texto

Rol que se asigna al cliente en SAP al momento de crearlo. Solo aplica cuando `contactModel` es `individualCustomers`.

> **Valor por defecto:** `CRM000`
