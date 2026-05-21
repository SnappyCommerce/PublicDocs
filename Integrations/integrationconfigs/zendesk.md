# Zendesk

## Variables

### `newContacts`
**Tipo:** booleano (true / false)

Si se activa, se crea automáticamente un ticket en Zendesk cada vez que un nuevo contacto inicia una conversación.

---

### `newTickets`
**Tipo:** booleano (true / false)

Si se activa, se crea automáticamente un ticket en Zendesk cada vez que se abre un nuevo ticket en el bot.

---

### `useZopim`
**Tipo:** booleano (true / false)

Habilita la integración con Zendesk Chat (anteriormente conocido como Zopim). Activar solo si la cuenta de Zendesk tiene el módulo de chat habilitado. Es una configuración del widget frontend.

---

### `autoInitializeChat`
**Tipo:** booleano (true / false)

Si se activa, el chat de Zendesk se abre automáticamente al iniciar la conversación, sin que el usuario lo solicite. Es una configuración del widget frontend.

---

### `zopimFirstMessage`
**Tipo:** texto

Mensaje inicial que se envía automáticamente al abrir el chat de Zendesk. Es una configuración del widget frontend.

---

### `brand`
**Tipo:** selector

Marca de Zendesk a la que se asocian los tickets creados. Útil cuando la cuenta de Zendesk tiene múltiples marcas/portales. Se selecciona de una lista que carga automáticamente — no requiere completar manualmente.
