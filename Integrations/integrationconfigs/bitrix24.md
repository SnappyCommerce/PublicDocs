# Bitrix24

## Variables

### `clientId` y `clientSecret`
**Tipo:** texto

Credenciales de la aplicación OAuth de Bitrix24. Son necesarios junto con `refreshToken` para autenticarse. Consultar con el equipo técnico del cliente cómo obtenerlos.

---

### `refreshToken`
**Tipo:** texto

Token OAuth que se usa para obtener acceso a la API de Bitrix24. Se actualiza automáticamente cada vez que se utiliza — no es necesario renovarlo manualmente.

---

### `marcaOrigen`
**Tipo:** texto

Valor que se guarda en un campo personalizado del CRM al crear un ticket/deal en Bitrix24. Identifica la marca o el origen del ticket.
