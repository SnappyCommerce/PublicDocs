# Freshchat

## Variables

### `domain`
**Tipo:** selector (`freshchat.com` / `freshworks.com`)

Dominio de la plataforma Freshchat usado por el widget frontend. Seleccionar según la URL que usa la cuenta del cliente.

---

### `subdomain`
**Tipo:** texto

Nombre de la cuenta del cliente en Freshchat. Es la parte inicial de la URL del panel.

> **Ejemplo:** si la URL es `miempresa.freshchat.com`, el subdomain es `miempresa`.

---

### `webhookUrl` y `webhookUrlTeam`
**Tipo:** texto (solo lectura)

URLs generadas automáticamente que deben copiarse y configurarse en el panel de Freshchat para que la integración reciba eventos. No requieren completarse manualmente.

---

### `groupId`, `webChannelId`, `whatsappChannelId`, `facebookMessengerChannelId`, `instagramChannelId`
**Tipo:** selector

Identificadores del grupo de agentes y los canales de Freshchat. Se seleccionan de listas que cargan automáticamente — no requieren completar manualmente.
