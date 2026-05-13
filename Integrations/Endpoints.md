# Integraciones — Documentación Completa para CX y Soporte

> Esta documentación cubre todo lo relacionado a cómo Snappy se conecta con plataformas externas: canales de mensajería, ecommerce, CRM, logística y herramientas de marketing. Incluye cómo funcionan los webhooks, los curls listos para verificar estados directamente en cada plataforma, y el mapeo completo de estados.

---

## 📋 Índice

1. [Cómo llegan los mensajes a Snappy (Canales)](#canales)
2. [Cómo se actualizan los pedidos (Webhooks)](#webhooks)
3. [Referencia de estados de Snappy](#estados-snappy)
4. [Endpoints y curls por integración](#endpoints)
   - VTEX · Shopify · Tienda Nube · WooCommerce · Mercado Pago · Mercado Libre · Shipnow · Magento 2.3 · Magento 2.4 · Bitrix24 · HubSpot · Salesforce · Zendesk · Freshdesk · Mailchimp · Doppler
5. [Verificar webhook activo](#verificar-webhook)
6. [Guía rápida de diagnóstico](#diagnostico)

---

## 📡 1. Cómo llegan los mensajes a Snappy (Canales) {#canales}

Cuando un usuario le escribe al bot de un cliente, el flujo es:

```
Usuario escribe → Canal (WA/Meta/ML/Email) → Chat-Server recibe el mensaje
→ Determina qué hacer → Bot responde o deriva a agente humano
```

### Canales soportados

| Canal | Descripción |
|---|---|
| **WhatsApp** | Vía Gupshup o integración directa. Soporta texto, imágenes, videos, documentos, audio, ubicación, contactos, botones, listas, reacciones y stickers |
| **Facebook Messenger** | Mensajes directos en páginas de Facebook |
| **Instagram Direct** | Mensajes directos en cuentas de Instagram |
| **Mercado Libre** | Consultas de compradores en publicaciones |
| **Email** | Recepción de emails vía inboxes configurados |

### WhatsApp — detalles

- Usa **Gupshup** como proveedor o integración directa
- El sistema cachea la configuración por **5 minutos** — si se cambia el número, puede tardar ese tiempo en reflejarse
- Configuración necesaria: número de teléfono del bot + App Name del proveedor

### Meta (Facebook e Instagram) — detalles

- Cada canal se configura con su propio **Page ID** y **Access Token**
- El tipo puede ser `messenger` (Facebook) o `instagram`
- Soporta: texto, imágenes, videos, stickers, reacciones, mensajes de voz, respuestas a historias

### Mercado Libre — detalles

- Recibe consultas de compradores (`questions`) y mensajes (`messages`) que se derivan al bot
- Los pedidos (`orders_v2`) se sincronizan si la tienda tiene activo el provider `syncOrders`
- Requiere OAuth: **Client ID**, **Client Secret** y **Redirect URI**

### Email — detalles

- Llega como evento `emailReceived` a un inbox configurado
- Los emails salientes se ignoran automáticamente
- Genera una notificación al agente con asunto y referencia del email

### Gestión de conversaciones

| Estado | Descripción |
|---|---|
| **Activa** | El bot está respondiendo |
| **Solicitando asistencia** | El bot o usuario pidió agente humano |
| **Con asistente asignado** | Un agente humano está atendiendo |
| **Finalizada** | La conversación fue cerrada |
| **Archivada** | La conversación finalizada fue archivada |

### Agentes humanos

1. El bot activa el flag de "solicitando asistencia"
2. El agente lo ve en el panel RATS y toma la conversación
3. El agente atiende y finaliza

> ⚠️ Una conversación finalizada puede archivarse pero **no reabrirse**. El usuario debe iniciar una nueva.

### Broadcast (envío masivo)

Permite enviar mensajes masivos de forma programada. Requiere que los usuarios hayan interactuado previamente con el bot dentro de la ventana de 24hs de WhatsApp, o usar plantillas aprobadas por Meta.

---

## 🔄 2. Cómo se actualizan los pedidos (Webhooks) {#webhooks}

Cuando ocurre un evento en una plataforma externa (ej: un pedido cambia de estado), esa plataforma le avisa automáticamente a Snappy:

```
Pedido cambia de estado en VTEX/Shopify/TN/ML
        ↓
La plataforma envía un webhook al servidor de Snappy
        ↓
El servidor valida el token, encola el evento
        ↓
Procesa en segundo plano: busca/crea contacto, actualiza pedido
        ↓
El bot puede informarle al usuario el nuevo estado
```

### Endpoints de webhooks

**Pedidos genérico (VTEX, Shopify, Tienda Nube):**
```
POST https://webhooks.snappylabs.io/webhook/orders/{linkToken}
```
El `linkToken` identifica de forma única a cada tienda.

**Mercado Libre:**
```
POST https://webhooks.snappylabs.io/webhook/mercadolibre
```

| Topic ML | ¿Qué hace Snappy? |
|---|---|
| `questions` / `messages` | Deriva al Chat-Server para que el bot responda |
| `orders_v2` | Sincroniza el pedido (solo si `syncOrders` está activo) |
| Cualquier otro | Lo acepta e ignora |

### Sistema de colas y seguridad

- Los eventos se encolan en Redis antes de procesarse → **no se pierden**
- **3 reintentos automáticos** si el procesamiento falla
- **Redis Lock** por pedido → el mismo pedido no se procesa dos veces simultáneamente
- Si llega un evento más antiguo que el último procesado, se ignora
- Si el `linkToken` es inválido → se rechaza el request

### Datos que sincroniza de cada pedido

- Productos (nombre, SKU, precio, cantidad, imagen, descuento)
- Totales (subtotal, total, costo de envío)
- Dirección de entrega
- Número externo del pedido
- Datos del comprador (nombre, email, teléfono)

---

## 📖 3. Referencia de estados de Snappy {#estados-snappy}

Cuando Snappy recibe un estado de una integración, lo traduce a su propio código interno:

| Código en Snappy | Significado |
|---|---|
| `created` | Pedido creado / confirmado |
| `validating` | Validando información |
| `approved` | Pedido aprobado |
| `processing` | En proceso (genérico) |
| `processing-delayed` | El procesamiento se demoró |
| `awaiting-user` | Esperando acción del usuario |
| `awaiting-pickup` | Esperando que el usuario retire |
| `invoiced` | Facturado |
| `awaiting-payment` | Esperando pago |
| `processing-payment` | Procesando pago |
| `payment-denied` | Pago rechazado |
| `payment-error` | Error en el pago |
| `preparing` | Preparando el pedido |
| `shipping` | Por enviarse |
| `shipped` | En camino |
| `shipping-to-branch` | En camino a sucursal |
| `shipping-to-pickup` | En camino a punto de retiro |
| `shipping-retry` | Reintentando envío |
| `shipping-delayed` | Envío demorado |
| `failed-to-deliver` | No se pudo entregar |
| `shipping-awaiting-external` | Esperando proveedor externo para enviar |
| `done` | Completado (genérico) |
| `picked-up` | Retirado por el usuario |
| `delivered` | Entregado |
| `devolution` | Devolución en curso |
| `processing-devolution` | Procesando devolución |
| `confirmed-devolution` | Devolución confirmada |
| `shipped-devolution` | Devolución en camino |
| `delivered-devolution` | Devolución entregada |
| `failed-to-deliver-devolution` | No se pudo entregar la devolución |
| `canceled` | Cancelado |
| `inexistent` | El pedido no existe |
| `error` | Error genérico |
| `unknown` | Pedido válido, sin información de estado |

> 💡 Si el estado crudo de la plataforma no aparece en la tabla de mapeo de esa integración, Snappy devuelve `unknown` o `inexistent`.

---

## 🔌 4. Endpoints y curls por integración {#endpoints}

> Reemplazá los valores entre `{llaves}` con los datos reales del cliente.
> Las credenciales están en **Panel Snappy → Integraciones → [nombre] → Configuración**.

---

### ⚙️ VTEX

Funcionalidades: búsqueda de productos · estado de pedido · categorías · especificaciones · contacto · checkout · factura · pedidos por email · promociones

**Variables:**
| Variable | Descripción |
|---|---|
| `{accountName}` | Nombre de la cuenta VTEX |
| `{environment}` | Dominio del entorno (ej: `myvtex.com`) |
| `{appKey}` | App Key de la integración |
| `{appToken}` | App Token de la integración |

#### 📦 Estado de un pedido

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/oms/pvt/orders/{orderId}" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

**Ejemplo:**
```bash
curl -X GET \
  "https://mitienda.myvtex.com/api/oms/pvt/orders/v70530116str-01" \
  -H "X-VTEX-API-AppKey: vtexappkey-mitienda-XXXXXX" \
  -H "X-VTEX-API-AppToken: XXXXXXXXXXXXXXXXXXXXXXXX"
```

**Campo:** `status` — Lógica especial: si el estado es `invoiced` con tracking → `shipped`; con retiro → `awaiting-pickup`; sin tracking → `done`

| Estado crudo VTEX | Código Snappy | Significado |
|---|---|---|
| `order-created` | `created` | Creado |
| `on-order-completed` | `processing` | En proceso |
| `payment-pending` | `awaiting-payment` | Esperando pago |
| `waiting-for-order-authorization` | `processing` | En proceso |
| `approve-payment` / `payment-approved` | `approved` | Aprobado |
| `payment-denied` | `payment-denied` | Pago rechazado |
| `request-cancel` / `cancellation-requested` / `cancel` / `canceled` | `canceled` | Cancelado |
| `waiting-for-seller-decision` / `authorize-fulfillment` / `invoice-after-cancellation-deny` | `processing` | En proceso |
| `order-create-error` / `order-creation-error` | `error` | Error |
| `window-to-cancel` | `shipping` | Por enviarse |
| `ready-for-handling` / `start-handling` / `handling` | `preparing` | Preparando |
| `invoice` / `invoiced` | `invoiced` | Facturado |
| `invoiced` (con tracking) | `shipped` | En camino |
| `invoiced` (pickup) | `awaiting-pickup` | Esperando retiro |
| `replaced` | `done` | Completado |
| Cualquier otro | `unknown` | Sin información |

#### 🧾 Factura de un pedido

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/oms/pvt/orders/{orderId}" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

> La URL de factura está en `packageAttachment.packages[].invoiceUrl`

#### 📋 Pedidos por email

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/oms/pvt/orders?q={email}&orderBy=creationDate,desc" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 🔍 Búsqueda de productos

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/catalog_system/pub/products/search?ft={término}&_from=0&_to=9" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 🗂️ Árbol de categorías

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/catalog_system/pub/category/tree/3" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 📋 Especificaciones de una categoría

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/catalog_system/pub/specification/field/listTreeByCategoryId/{categoryId}" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 👤 Buscar cliente (MasterData)

**MasterData V1:**
```bash
curl -X GET \
  "https://{accountName}.{environment}/api/dataentities/CL/search?email={email}&_fields=_all" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

**MasterData V2:**
```bash
curl -X GET \
  "https://{accountName}.{environment}/api/dataentities/Cliente/search?email={email}&_fields=_all" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 🎁 Promociones activas

```bash
curl -X GET \
  "https://{accountName}.{environment}/api/rnb/pvt/benefits/calculatorconfiguration" \
  -H "X-VTEX-API-AppKey: {appKey}" \
  -H "X-VTEX-API-AppToken: {appToken}"
```

#### 🛍️ Generar checkout (4 pasos)

```bash
# Paso 1 — Crear carrito
curl -X GET "https://{accountName}.{environment}/api/checkout/pub/orderForm?forceNewCart=true" \
  -H "X-VTEX-API-AppKey: {appKey}" -H "X-VTEX-API-AppToken: {appToken}"

# Paso 2 — Datos del cliente (usar {orderFormId} del paso 1)
curl -X POST "https://{accountName}.{environment}/api/checkout/pub/orderForm/{orderFormId}/attachments/clientProfileData" \
  -H "X-VTEX-API-AppKey: {appKey}" -H "X-VTEX-API-AppToken: {appToken}" \
  -H "Content-Type: application/json" \
  -d '{"email":"{email}","firstName":"{nombre}","lastName":"{apellido}","phone":"{telefono}"}'

# Paso 3 — Agregar productos
curl -X POST "https://{accountName}.{environment}/api/checkout/pub/orderForm/{orderFormId}/items" \
  -H "X-VTEX-API-AppKey: {appKey}" -H "X-VTEX-API-AppToken: {appToken}" \
  -H "Content-Type: application/json" \
  -d '{"orderItems":[{"id":"{skuId}","quantity":1,"seller":"1"}]}'

# Paso 4 — Dirección de envío
curl -X POST "https://{accountName}.{environment}/api/checkout/pub/orderForm/{orderFormId}/attachments/shippingData" \
  -H "X-VTEX-API-AppKey: {appKey}" -H "X-VTEX-API-AppToken: {appToken}" \
  -H "Content-Type: application/json" \
  -d '{"address":{"addressType":"residential","receiverName":"{nombre}","street":"{calle}","number":"{numero}","city":"{ciudad}","state":"{provincia}","country":"ARG","postalCode":"{cp}"}}'
```

> Link de checkout final: `https://{url-tienda}/checkout?orderFormId={orderFormId}`

---

### 🛍️ Shopify

Funcionalidades: estado de pedido · búsqueda de productos · categorías · checkout

**Variables:** `{shop}` (ej: `mitienda.myshopify.com`), `{accessToken}`

#### 📦 Estado de un pedido

```bash
curl -X GET \
  "https://{shop}/admin/api/2020-07/orders.json?name={orderId}&status=any" \
  -H "X-Shopify-Access-Token: {accessToken}"
```

**Campos:** `financial_status`, `fulfillments[0].shipment_status`, `cancel_reason`, `cancelled_at`

| Condición Shopify | Código Snappy | Significado |
|---|---|---|
| `cancel_reason` o `cancelled_at` presentes | `canceled` | Cancelado |
| `financial_status: pending` | `processing-payment` | Procesando pago |
| `shipment_status: confirmed` | `approved` | Aprobado |
| `shipment_status: in_transit` | `shipped` | En camino |
| `shipment_status: out_for_delivery` | `shipping` | Por enviarse |
| `shipment_status: delivered` / `fulfilled` | `delivered` | Entregado |
| `shipment_status: ready_for_pickup` | `awaiting-pickup` | Esperando retiro |
| `shipment_status: label_printed` / `label_purchased` | `preparing` | Preparando |
| `shipment_status: attempted_delivery` | `shipping-retry` | Reintentando |
| `shipment_status: partial` | `shipped` | En camino parcial |
| Sin fulfillment | `inexistent` | Sin estado |

#### 🔍 Búsqueda de productos (GraphQL)

```bash
curl -X POST "https://{shop}/admin/api/2024-10/graphql.json" \
  -H "X-Shopify-Access-Token: {accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"query":"{ products(first: 10, query: \"{término}\") { edges { node { id title variants(first: 5) { edges { node { id title price sku } } } } } } }"}'
```

#### 🗂️ Categorías (GraphQL)

```bash
curl -X POST "https://{shop}/admin/api/2024-10/graphql.json" \
  -H "X-Shopify-Access-Token: {accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"query":"{ collections(first: 50) { edges { node { id title handle } } } }"}'
```

#### 🛍️ Generar checkout (Draft Order, GraphQL)

```bash
curl -X POST "https://{shop}/admin/api/2024-10/graphql.json" \
  -H "X-Shopify-Access-Token: {accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"query":"mutation draftOrderCreate($input: DraftOrderInput!) { draftOrderCreate(input: $input) { draftOrder { id invoiceUrl } userErrors { field message } } }","variables":{"input":{"lineItems":[{"variantId":"gid://shopify/ProductVariant/{variantId}","quantity":1}],"email":"{email}","shippingAddress":{"firstName":"{nombre}","lastName":"{apellido}","address1":"{calle numero}","city":"{ciudad}","zip":"{cp}","countryCode":"AR"}}}}'
```

---

### ☁️ Tienda Nube

Funcionalidades: estado de pedido · productos · categorías · cliente · sincronizar contactos · checkout · carritos abandonados · notificaciones de stock

**Variables:** `{userId}`, `{accessToken}`

#### 📦 Estado de un pedido

```bash
curl -X GET \
  "https://api.tiendanube.com/v1/{userId}/orders/{orderId}" \
  -H "Authentication: bearer {accessToken}"
```

**Campos:** `status`, `payment_status`, `shipping_status`, `next_action`

| `status` | `shipping_status` / condición | Código Snappy | Significado |
|---|---|---|---|
| `open` | `shipping_status: unpacked` | `preparing` | Preparando |
| `open` | `unpacked` + `payment_status: pending` | `awaiting-payment` | Esperando pago |
| `open` | `shipping_status: unshipped` | `preparing` | Preparando |
| `open` | `unshipped` + `next_action: waiting_client_pickup` | `awaiting-pickup` | Esperando retiro |
| `open` | `shipping_status: shipped` / `fulfilled` / `partially_fulfilled` | `shipped` | En camino |
| `open` | `shipping_status: partially_packed` | `shipping` | Por enviarse |
| `open` | `shipping_status: delivered` | `delivered` | Entregado |
| `closed` | — | `delivered` | Entregado |
| `canceled` / `archived` | — | `canceled` | Cancelado |
| `open` (sin shipping) | — | `created` | Creado |
| Otro | — | `unknown` | Sin información |

#### 🔍 Búsqueda de productos

```bash
curl -X GET \
  "https://api.tiendanube.com/v1/{userId}/products?q={término}&per_page=10&page=1" \
  -H "Authentication: bearer {accessToken}"
```

#### 🗂️ Categorías

```bash
curl -X GET "https://api.tiendanube.com/v1/{userId}/categories" \
  -H "Authentication: bearer {accessToken}"
```

#### 👤 Obtener / buscar cliente

```bash
# Por ID
curl -X GET "https://api.tiendanube.com/v1/{userId}/customers/{customerId}" \
  -H "Authentication: bearer {accessToken}"

# Buscar por email
curl -X GET "https://api.tiendanube.com/v1/{userId}/customers?q={email}" \
  -H "Authentication: bearer {accessToken}"
```

#### 🛒 Carritos abandonados

```bash
curl -X GET \
  "https://api.tiendanube.com/v1/{userId}/checkouts?updated_at_min={fecha_iso}&updated_at_max={fecha_iso}&per_page=20" \
  -H "Authentication: bearer {accessToken}"
```

#### 🔔 Notificaciones de stock (StockNube)

```bash
curl -X POST "https://www.stocknube.app/api/notifications" \
  -H "Content-Type: application/json" \
  -d '{"productId":"{productId}","productVariantId":"{variantId}","productName":"{nombre}","storeId":"{userId}","lang":"es","email":"{emailUsuario}"}'
```

#### 🛍️ Generar checkout

```bash
curl -X POST "https://api.tiendanube.com/v1/{userId}/checkouts" \
  -H "Authentication: bearer {accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"products":[{"variant_id":"{variantId}","quantity":1}],"contact_name":"{nombre}","contact_lastname":"{apellido}","contact_email":"{email}","contact_phone":"{telefono}","sale_channel":"Snappy"}'
```

---

### 🛒 WooCommerce

Funcionalidades: estado de pedido · productos · variaciones · categorías

**Variables:** `{url}`, `{consumerKey}`, `{consumerSecret}`

#### 📦 Estado de un pedido

```bash
curl -X GET "https://{url}/wp-json/wc/v3/orders/{orderId}" \
  -u "{consumerKey}:{consumerSecret}"
```

| Estado WooCommerce | Código Snappy | Significado |
|---|---|---|
| `pending` | `validating` | Validando |
| `processing` | `processing` | En proceso |
| `on-hold` | `preparing` | Preparando |
| `completed` | `done` | Completado |
| `cancelled` | `canceled` | Cancelado |
| `refunded` | `devolution` | Devolución |
| `failed` / `trash` | `error` | Error |
| Otro | `unknown` | Sin información |

#### 🔍 Búsqueda de productos

```bash
curl -X GET "https://{url}/wp-json/wc/v3/products?search={término}&per_page=10" \
  -u "{consumerKey}:{consumerSecret}"
```

#### 🔍 Variaciones de un producto

```bash
curl -X GET "https://{url}/wp-json/wc/v3/products/{productId}/variations" \
  -u "{consumerKey}:{consumerSecret}"
```

#### 🗂️ Categorías

```bash
curl -X GET "https://{url}/wp-json/wc/v3/products/categories?per_page=50" \
  -u "{consumerKey}:{consumerSecret}"
```

---

### 💰 Mercado Pago

Funcionalidades: generar checkout

> ⚠️ Token OAuth — se renueva automáticamente. Si devuelve `401`, el cliente debe reconectar desde el panel de Snappy.

#### 🛍️ Generar preferencia de pago

```bash
curl -X POST "https://api.mercadopago.com/checkout/preferences" \
  -H "Authorization: Bearer {accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"items":[{"id":"{productId}","title":"{nombre}","quantity":1,"unit_price":100,"currency_id":"ARS"}],"payer":{"name":"{nombre}","email":"{email}"}}'
```

> El link de checkout al usuario es `init_point` en la respuesta.

---

### 🟡 Mercado Libre

Funcionalidades: estado de pedido (sincronización via webhook) · comentarios en publicaciones

> ⚠️ Token OAuth — se renueva automáticamente. Si devuelve `401`, el cliente debe reconectar desde el panel de Snappy.

#### 📦 Estado de un pedido

```bash
curl -X GET "https://api.mercadolibre.com/orders/{orderId}" \
  -H "Authorization: Bearer {accessToken}"
```

| Estado crudo ML | Código Snappy | Significado |
|---|---|---|
| `confirmed` | `opened` | Abierto |
| `payment_required` / `payment_in_process` / `partially_paid` | `paymentPending` | Pago pendiente |
| `paid` | `paymentApproved` | Pago aprobado |
| `partially_refunded` | `paymentRefunded` | Pago reembolsado |
| `pending_cancel` | `cancellationRequested` | Cancelación solicitada |
| `cancelled` / `invalid` | `cancelled` | Cancelado |
| Otro | `opened` | Abierto (default) |

#### 👤 Datos del comprador

```bash
curl -X GET "https://api.mercadolibre.com/users/{userId}" \
  -H "Authorization: Bearer {accessToken}"
```

---

### 🚀 Shipnow

Funcionalidades: actualización de estado de envíos (solo actualiza pedidos existentes, no crea nuevos)

**Variable:** `{token}`

#### 📦 Estado de un envío

```bash
curl -X GET "https://api.shipnow.com.ar/orders/{orderId}" \
  -H "Authorization: Bearer {token}"

# Si tiene sub-envíos
curl -X GET "https://api.shipnow.com.ar/orders?main_order_id={orderId}" \
  -H "Authorization: Bearer {token}"
```

| `status` | `service_mode` | `service_code` | Código Snappy | Significado |
|---|---|---|---|---|
| `new` | normal | — | `created` | Creado |
| `awaiting_payment` | normal | — | `awaiting-payment` | Esperando pago |
| `ready_to_pick` / `ready_to_pack` | normal | — | `preparing` | Preparando |
| `ready_to_ship` | normal | — | `shipping` | Por enviarse |
| `on_hold` | normal | — | `processing` | En proceso |
| `shipped` | normal | (no pickup) | `shipped` | En camino |
| `shipped` | normal | `pickup_store` | `shipping-to-branch` | En camino a sucursal |
| `delivered` | normal | (no pickup) | `delivered` | Entregado |
| `delivered` | normal | `pickup_store` | `awaiting-pickup` | Esperando retiro |
| `not_delivered` | normal | — | `failed-to-deliver` | No se pudo entregar |
| `return` | normal | — | `devolution` | Devolución |
| `cancelled` | normal | — | `canceled` | Cancelado |
| `new` | `exchange` | — | `confirmed-devolution` | Devolución confirmada |
| `shipped` | `exchange` | — | `shipped-devolution` | Devolución en camino |
| `delivered` | `exchange` | — | `delivered-devolution` | Devolución entregada |
| `not_delivered` | `exchange` | — | `failed-to-deliver-devolution` | Devolución no entregada |

---

### 🏗️ Magento 2.3

Funcionalidades: estado de pedido · productos · categorías · carritos abandonados · factura

**Variables:** `{url}`, `{accessToken}`

#### 📦 Estado de un pedido

```bash
curl -X GET \
  "https://{url}/index.php/rest/V1/orders?searchCriteria[filter_groups][0][filters][0][field]=increment_id&searchCriteria[filter_groups][0][filters][0][value]={orderId}&searchCriteria[filter_groups][0][filters][0][condition_type]=like&searchCriteria[pageSize]=20" \
  -H "Authorization: Bearer {accessToken}"
```

> Campo a revisar: `status` dentro de `items[0]`. Si el método de envío es `instore_pickup`, `complete` mapea a `preparing` en vez de `shipped`.

| Estado crudo Magento 2.3 | Código Snappy | Significado |
|---|---|---|
| `processing` | `processing` | En proceso |
| `fraud` | `error` | Error |
| `pending_payment` / `pending_paypal` | `awaiting-payment` | Esperando pago |
| `payment_review` | `processing-payment` | Procesando pago |
| `pending` | `validating` | Validando |
| `holded` | `shipping-awaiting-external` | Esperando proveedor externo |
| `STATE_OPEN` | `shipped` | En camino |
| `complete` (envío normal) | `shipped` | En camino |
| `complete` (retiro) | `preparing` | Preparando |
| `closed` | `done` | Completado |
| `canceled` | `canceled` | Cancelado |
| `paypay_canceled_reversal` / `paypal_reversed` | `payment-error` | Error en pago |
| `facturado` | `invoiced` | Facturado |
| `aprobado` / `pre_order` | `approved` | Aprobado |
| `en_preparacion` / `preparacion_processing` | `preparing` | Preparando |
| `en_transito` / `despachado` / `sent` / `STATE_OPEN` | `shipped` | En camino |
| `listo_para_enviar_complete` | `shipping` | Por enviarse |
| `listo_para_retiro` / `listo_retiro_complete` | `awaiting-pickup` | Esperando retiro |
| `entregado_complete` | `delivered` | Entregado |
| `andreani_shipment_failed` | `failed-to-deliver` | No se pudo entregar |
| Otro | `inexistent` | No mapeado |

#### 🔍 Búsqueda de productos

```bash
curl -X GET \
  "https://{url}/index.php/rest/V1/products?searchCriteria[filter_groups][0][filters][0][field]=name&searchCriteria[filter_groups][0][filters][0][value]=%25{término}%25&searchCriteria[filter_groups][0][filters][0][condition_type]=like&searchCriteria[pageSize]=10" \
  -H "Authorization: Bearer {accessToken}"
```

#### 🗂️ Categorías

```bash
curl -X GET "https://{url}/index.php/rest/V1/categories" \
  -H "Authorization: Bearer {accessToken}"
```

#### 🛒 Carritos abandonados

```bash
curl -X GET \
  "https://{url}/index.php/rest/V1/carts/search?searchCriteria[filter_groups][0][filters][0][field]=updated_at&searchCriteria[filter_groups][0][filters][0][value]={fecha_iso}&searchCriteria[filter_groups][0][filters][0][condition_type]=gt&searchCriteria[pageSize]=20" \
  -H "Authorization: Bearer {accessToken}"
```

---

### 🏗️ Magento 2.4

Funcionalidades: productos · categorías · facetas

> ⚠️ Magento 2.4 **no tiene estado de pedido** en Snappy. Solo soporta búsqueda de productos, categorías y facetas.

**Variables:** `{url}`, `{accessToken}`

#### 🔍 Búsqueda de productos

```bash
curl -X GET \
  "https://{url}/index.php/rest/V1/products?searchCriteria[filter_groups][0][filters][0][field]=name&searchCriteria[filter_groups][0][filters][0][value]=%25{término}%25&searchCriteria[filter_groups][0][filters][0][condition_type]=like&searchCriteria[pageSize]=20" \
  -H "Authorization: Bearer {accessToken}"
```

#### 🗂️ Categorías

```bash
curl -X GET "https://{url}/index.php/rest/V1/categories" \
  -H "Authorization: Bearer {accessToken}"
```

#### 🎛️ Facetas / Filtros disponibles

```bash
curl -X GET \
  "https://{url}/index.php/rest/V1/products/attributes?searchCriteria[filter_groups][0][filters][0][field]=is_filterable&searchCriteria[filter_groups][0][filters][0][value]=1&searchCriteria[pageSize]=50" \
  -H "Authorization: Bearer {accessToken}"
```

---

### 🟠 Bitrix24

Funcionalidades: crear contacto · crear ticket (Deal)

> Token OAuth con refresh automático. Snappy renueva el token antes de cada operación.

**Variables:** `{domain}`, `{clientId}`, `{clientSecret}`, `{refreshToken}`, `{marcaOrigen}`

#### 🔑 Obtener Access Token

```bash
curl -X GET \
  "{domain}/oauth/token/?grant_type=refresh_token&client_id={clientId}&client_secret={clientSecret}&refresh_token={refreshToken}&scope=user,crm"
```

#### 👤 Buscar contacto

```bash
curl -X POST "{domain}/rest/crm.contact.list?auth={accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"filter":{"EMAIL":"{email}","PHONE":"{telefono}"}}'
```

#### 👤 Crear contacto

```bash
curl -X POST "{domain}/rest/crm.contact.add?auth={accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"fields":{"NAME":"{nombre}","LAST_NAME":"{apellido}","PHONE":[{"VALUE":"{telefono}","VALUE_TYPE":"WORK"}],"EMAIL":[{"VALUE":"{email}","VALUE_TYPE":"WORK"}]}}'
```

#### 🎫 Crear ticket (Deal)

```bash
curl -X POST "{domain}/rest/crm.deal.add?auth={accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"fields":{"TITLE":"{titulo}","COMMENTS":"{descripcion}","CONTACT_IDS":["{bitrixContactId}"],"UF_CRM_5F41B3A5BBEA3":{marcaOrigen}}}'
```

#### 🔍 Ver tickets de un contacto

```bash
curl -X POST "{domain}/rest/crm.deal.list?auth={accessToken}" \
  -H "Content-Type: application/json" \
  -d '{"filter":{"CONTACT_ID":"{contactId}"},"select":["*","EMAIL","PHONE"]}'
```

---

### 🟣 HubSpot

Funcionalidades: crear/actualizar contacto · crear ticket

**Variable:** `{apiKey}`

#### 👤 Buscar contacto por email

```bash
curl -X GET \
  "https://api.hubapi.com/crm/v3/objects/contacts/{email}?idProperty=email" \
  -H "Authorization: Bearer {apiKey}"
```

#### 👤 Crear contacto

```bash
curl -X POST "https://api.hubapi.com/crm/v3/objects/contacts" \
  -H "Authorization: Bearer {apiKey}" -H "Content-Type: application/json" \
  -d '{"properties":{"email":"{email}","firstname":"{nombre}","phone":"{telefono}"}}'
```

#### 👤 Actualizar contacto

```bash
curl -X PATCH "https://api.hubapi.com/crm/v3/objects/contacts/{contactId}" \
  -H "Authorization: Bearer {apiKey}" -H "Content-Type: application/json" \
  -d '{"properties":{"email":"{email}","firstname":"{nombre}","phone":"{telefono}"}}'
```

#### 🎫 Crear ticket

```bash
curl -X POST "https://api.hubapi.com/crm/v3/objects/tickets" \
  -H "Authorization: Bearer {apiKey}" -H "Content-Type: application/json" \
  -d '{"properties":{"subject":"{titulo}","hs_ticket_priority":"MEDIUM"},"associations":[{"to":{"id":"{contactId}"},"types":[{"associationCategory":"HUBSPOT_DEFINED","associationTypeId":16}]}]}'
```

> Prioridades: `HIGH`, `MEDIUM`, `LOW`

---

### ☁️ Salesforce

Funcionalidades: crear caso (ticket)

**Variables:** `{instanceUrl}`, `{username}`, `{password}`, `{clientId}`, `{clientSecret}`, `{token}`, `{grantType}`

#### 🔑 Obtener Access Token

```bash
curl -X POST "{instanceUrl}/services/oauth2/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type={grantType}&client_id={clientId}&client_secret={clientSecret}&username={username}&password={password}{token}"
```

#### 🎫 Crear caso

```bash
curl -X POST "{instanceUrl}/services/data/v51.0/sobjects/Case" \
  -H "Authorization: Bearer {accessToken}" -H "Content-Type: application/json" \
  -d '{"Subject":"Snappy - {titulo}","Description":"{descripcion}","Nombres__c":"{nombre}","SuppliedEmail":"{email}"}'
```

#### 🔍 Ver caso por ID

```bash
curl -X GET "{instanceUrl}/services/data/v51.0/sobjects/Case/{caseId}" \
  -H "Authorization: Bearer {accessToken}"
```

---

### 🟢 Zendesk

Funcionalidades: crear contacto (ticket Lead) · crear ticket · actualizar ticket · transferencia de chat

**Variables:** `{subdomain}`, `{email}`, `{token}`
> Auth: `-u "{email}/token:{token}"`

#### 👤 Crear contacto (ticket tipo Lead)

```bash
curl -X POST "https://{subdomain}.zendesk.com/api/v2/tickets.json" \
  -u "{email}/token:{token}" -H "Content-Type: application/json" \
  -d '{"ticket":{"subject":"Snappy - New Client - {nombre}","type":"question","comment":{"body":"{transcripcion}"},"tags":["Snappy","Lead"],"requester":{"email":"{emailUsuario}","name":"{nombreUsuario}"}}}'
```

#### 🎫 Crear ticket

```bash
curl -X POST "https://{subdomain}.zendesk.com/api/v2/tickets.json" \
  -u "{email}/token:{token}" -H "Content-Type: application/json" \
  -d '{"ticket":{"subject":"{titulo}","type":"question","comment":{"body":"{descripcion}"},"tags":["Snappy","Ticket"],"requester":{"email":"{emailUsuario}","name":"{nombreUsuario}"}}}'
```

#### ✏️ Actualizar ticket

```bash
curl -X PUT "https://{subdomain}.zendesk.com/api/v2/tickets/{ticketId}.json" \
  -u "{email}/token:{token}" -H "Content-Type: application/json" \
  -d '{"ticket":{"comment":{"body":"{comentario}"},"status":"open"}}'
```

#### 🔍 Ver ticket

```bash
curl -X GET "https://{subdomain}.zendesk.com/api/v2/tickets/{ticketId}.json" \
  -u "{email}/token:{token}"
```

---

### 🔵 Freshdesk

Funcionalidades: crear contacto · crear ticket · actualizar ticket · transferencia de chat

**Variables:** `{subdomain}`, `{apiKey}`
> Auth: `-u "{apiKey}:X"` (la contraseña siempre es `X`)

#### 👤 Crear contacto

```bash
curl -X POST "https://{subdomain}.freshdesk.com/api/v2/contacts" \
  -u "{apiKey}:X" -H "Content-Type: application/json" \
  -d '{"name":"{nombre}","email":"{email}","phone":"{telefono}"}'
```

#### 🎫 Crear ticket

```bash
curl -X POST "https://{subdomain}.freshdesk.com/api/v2/tickets" \
  -u "{apiKey}:X" -H "Content-Type: application/json" \
  -d '{"subject":"{titulo}","description":"{descripcion}\nSnappy Ticket: {friendlyId}","email":"{emailUsuario}","status":2,"priority":2,"tags":["Ticket","SnappyLabs","Chat"]}'
```

> **Status:** `2`=Abierto · `3`=Pendiente · `4`=Resuelto · `5`=Cerrado
> **Priority:** `1`=Bajo · `2`=Medio · `3`=Alto · `4`=Urgente

#### ✏️ Actualizar ticket

```bash
curl -X PUT "https://{subdomain}.freshdesk.com/api/v2/tickets/{ticketId}" \
  -u "{apiKey}:X" -H "Content-Type: application/json" \
  -d '{"status":3,"priority":2}'
```

#### 🔍 Ver grupos / productos

```bash
curl -X GET "https://{subdomain}.freshdesk.com/api/v2/groups?page=1" -u "{apiKey}:X"
curl -X GET "https://{subdomain}.freshdesk.com/api/v2/products?page=1" -u "{apiKey}:X"
```

---

### 📧 Mailchimp

Funcionalidades: agregar/actualizar contacto en lista

**Variables:** `{dc}` (ej: `us1`), `{apiKey}`, `{listId}`
> El identificador del suscriptor es el MD5 del email en minúsculas.

#### 👤 Agregar o actualizar contacto

```bash
curl -X PUT \
  "https://{dc}.api.mailchimp.com/3.0/lists/{listId}/members/{md5_del_email}" \
  -u "anystring:{apiKey}" -H "Content-Type: application/json" \
  -d '{"email_address":"{email}","status_if_new":"subscribed","merge_fields":{"FNAME":"{nombre}","LNAME":"{apellido}"}}'
```

#### 📋 Ver listas disponibles

```bash
curl -X GET "https://{dc}.api.mailchimp.com/3.0/lists?count=100" \
  -u "anystring:{apiKey}"
```

---

### 🔴 Doppler

Funcionalidades: agregar/actualizar suscriptor en lista

**Variables:** `{accountName}`, `{apiKey}`, `{listId}`

#### 👤 Agregar o actualizar suscriptor

```bash
curl -X PUT \
  "https://restapi.fromdoppler.com/accounts/{accountName}/lists/{listId}/subscribers" \
  -H "Authorization: token {apiKey}" -H "Content-Type: application/json" \
  -d '{"email":"{email}","fields":[{"name":"FIRSTNAME","value":"{nombre}","predefined":true,"private":true,"readonly":true,"type":"string"},{"name":"LASTNAME","value":"{apellido}","predefined":true,"private":true,"readonly":true,"type":"string"}]}'
```

#### 📋 Ver listas disponibles

```bash
curl -X GET "https://restapi.fromdoppler.com/accounts/{accountName}/lists" \
  -H "Authorization: token {apiKey}"
```

---

## 🔗 5. Verificar que el webhook de Snappy está activo {#verificar-webhook}

```bash
curl -X POST \
  "https://webhooks.snappylabs.io/webhook/orders/{linkToken}" \
  -H "Content-Type: application/json" \
  -d '{"hookConfig": "ping"}'
```

**Respuesta si está activo:**
```json
{ "code": "pong", "message": "Pong!" }
```

**Respuesta si el token es inválido:**
```json
{ "code": "invalid_token", "message": "Invalid Link Token" }
```

---

## ❓ 6. Guía rápida de diagnóstico {#diagnostico}

Cuando el estado en Snappy no coincide con el de la plataforma:

1. Obtener el ID del pedido desde Snappy o del usuario
2. Ejecutar el curl correspondiente con las credenciales del cliente
3. Comparar el estado crudo con la tabla de mapeo de esa integración
4. Si debería haberse actualizado y no lo hizo:
   - Verificar si el webhook responde con el curl de ping
   - El webhook se reintenta 3 veces automáticamente si falla
   - Para ML y MP: si el curl devuelve `401` → token expirado → reconectar desde el panel
   - Si las credenciales de la API cambiaron → actualizarlas en el panel de Snappy
   - Si el estado crudo no está en la tabla de mapeo → el estado no está implementado, Snappy devuelve `unknown`

---

*Documentación generada el 05/05/2026 — Basada en Common-Back, Chat-Server y Webhooks*
