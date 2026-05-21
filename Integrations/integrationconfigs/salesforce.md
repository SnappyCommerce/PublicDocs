# Salesforce

## Variables

### `instanceUrl`
**Tipo:** texto

URL de la instancia de Salesforce del cliente.

> **Ejemplo:** `https://miempresa.my.salesforce.com`

---

### `clientId` y `clientSecret`
**Tipo:** texto

Credenciales de la aplicación OAuth de Salesforce. Son necesarios para el flujo de autenticación junto con `username`, `password` y `token`. Consultar con el equipo técnico del cliente cómo obtenerlos.

---

### `username` y `password`
**Tipo:** texto

Credenciales del usuario de Salesforce que se usa para autenticarse. El `password` se combina internamente con el `token` para completar la autenticación.

---

### `token`
**Tipo:** texto

Security Token de Salesforce del usuario. No es un API token genérico — es un código adicional que Salesforce combina con el `password` para autenticar. Se obtiene desde la configuración del perfil de usuario en Salesforce (Configuración → Mis datos personales → Restablecer mi token de seguridad).

---

### `grantType`
**Tipo:** texto

Tipo de autenticación OAuth utilizado. En la mayoría de los casos el valor es `password`.
