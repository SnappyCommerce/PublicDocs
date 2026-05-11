# TemplateV2 Bot Reference

---

## Table of Contents

- [Configuration Reference](#configuration-reference)
  - [Core](#core)
    - [Top-level keys](#top-level-keys)
    - [flags](#flags)
  - [Features](#features)
    - [accountPoints](#accountpoints)
    - [addProductToCart](#addproducttocart)
    - [branches](#branches)
    - [createContact](#createcontact)
    - [createTicket](#createticket)
    - [generateCheckout](#generatecheckout)
    - [humanAssistance](#humanassistance)
    - [orderStatus](#orderstatus)
    - [productSearch](#productsearch)
    - [promotions](#promotions)
  - [AI & Orchestrator](#ai--orchestrator)
    - [awi](#awi)
    - [awk](#awk)
    - [gms](#gms)
    - [orchestratorFunctions](#orchestratorfunctions)
    - [orchestratorOverwrites](#orchestratoroverwrites)
    - [splitTesting](#splittesting)
- [Custom Flows](#custom-flows)
  - [How it works](#how-it-works)
  - [Defining a custom flow](#defining-a-custom-flow)
  - [Calling a flow](#calling-a-flow-dynamicflow-parameters)
  - [Connecting a flow to the orchestrator](#connecting-a-flow-to-the-orchestrator)
- [Flow Functions Reference](#flow-functions-reference)
  - [awk — Answer With Knowledge](#awk--answer-with-knowledge)
  - [awp — Answer With Product](#awp--answer-with-product)
  - [assistance — Human Assistance](#assistance--human-assistance)
  - [ticket — Create Ticket](#ticket--create-ticket)
  - [contact — Create Contact](#contact--create-contact)
  - [orderStatus — Order Status](#orderstatus--order-status)
  - [productDiscovery — Product Discovery](#productdiscovery--product-discovery)
  - [branches — Branches](#branches--branches)
  - [promotions — Promotions](#promotions--promotions)
  - [addProductToCart — Add Product To Cart](#addproducttocart--add-product-to-cart)
  - [accountPoints — Account Points](#accountpoints--account-points)
  - [faq — FAQ](#faq--faq)
  - [menu — Show Menu](#menu--show-menu)
  - [sector — Set Sector](#sector--set-sector)
  - [topic — Set Topic](#topic--set-topic)
  - [survey — Satisfaction Survey](#survey--satisfaction-survey)
  - [ratingSurveyReply — Rating Survey Reply](#ratingsurveyreply--rating-survey-reply)
  - [stockNotification — Stock Notification](#stocknotification--stock-notification)
  - [notify — Notify](#notify--notify)
  - [llm — LLM (V3)](#llm--llm-v3)

---

# Configuration Reference

All keys below are properties of the `$config` context variable. A key listed as `foo.bar` is accessed as `$config.foo.bar`.

---

## Core

---

### Top-level keys

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `debug` | boolean | `false` | Enables debug mode (also toggled at runtime via `debug:enable` input) |
| `version` | string | — | Bot version. `"v3"` enables the V3 orchestrator and AWI path; anything else falls back to V2 behavior |
| `mainMenu` | string | — | IVR menu key used by the proactive message flow to display a main menu |
| `disabledChannels` | string[] | `[]` | List of channel identifiers on which the bot is fully disabled (e.g. `["facebook", "instagram"]`) |
| `disableIntentClassifier` | boolean | `false` | Bypasses the intent classifier entirely; all routing is handled by the orchestrator directly |
| `forcedTermsAndConditions` | boolean | `false` | Forces users to accept Terms & Conditions before any flow proceeds. Also accepted as `flags.forcedTermsAndConditions` |

---

## Features

---

### `accountPoints`

Loyalty/points account settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `accountPoints.shouldRequestConsent` | boolean | `false` | When `true`, asks the user for consent before accessing their points account data |

---

### `addProductToCart`

Add-to-cart flow settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `addProductToCart.defaultQuantity` | number | `1` | Default quantity used when the user does not specify an amount |

---

### `branches`

Branch/store locator flow settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `branches.mode` | string | `"default"` | Controls how the branch search operates. Options: `"default"` (standard keyword search), `"orchestrator"` (LLM-assisted search), `"knowledge"` (vector knowledge search), `"full"` (knowledge + orchestrator fallback) |

---

### `createContact`

Contact record creation settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `createContact.enabled` | boolean | `true` | Enables the contact creation flow |
| `createContact.requirements` | string[] | `["email", "firstName", "lastName", "phone"]` | Contact fields that must be collected before creating a record |
| `createContact.additionalFields` | object[] | `[]` | Extra fields to include in the contact creation payload |
| `createContact.autoCreateContact` | boolean | `true` | Automatically creates the contact as soon as all requirements are collected |
| `createContact.useSnappyData` | boolean | `true` | Pre-fills contact fields from existing Snappy user data when available |

---

### `createTicket`

Support ticket creation settings. Supports named sub-configurations (e.g. `createTicket.fileAClaim`) selectable at call time via `$createTicket.config`.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `createTicket.enabled` | boolean | `true` | Enables ticket creation |
| `createTicket.notification` | string | `"The user will be contacted shortly"` | Message shown to the user after a ticket is successfully created |
| `createTicket.isExternalIdMandatory` | boolean | `false` | When `true`, ticket creation fails if no external ticket ID is returned by the provider |
| `createTicket.respondWithExternalId` | boolean | `false` | When `true`, shows the external ticket ID to the user in the confirmation message |
| `createTicket.externalTicketFallbackMessage` | string | — | Message shown if the external ticket ID is mandatory but was not returned |
| `createTicket.default.requirements` | string[] | `["ticketReason", "ticketSubreason"]` | Fields required before creating a ticket (used when no named config is specified) |
| `createTicket.default.additionalFields` | object[] | `[]` | Extra fields to include in the ticket payload for the default config |
| `createTicket.default.optionals` | string[] | `["files", "comment"]` | Optional fields the user may provide |
| `createTicket.default.MinFilesAmount` | string | `""` | Minimum number of file attachments required (empty = no minimum) |
| `createTicket.<name>.requirements` | string[] | — | Requirements for a named ticket sub-configuration (e.g. `createTicket.fileAClaim.requirements`) |
| `createTicket.<name>.additionalFields` | object[] | — | Additional fields for a named ticket sub-configuration |

---

### `generateCheckout`

Checkout URL generation settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `generateCheckout.enabled` | boolean | `true` | Enables checkout generation |
| `generateCheckout.requirements.contact` | string[] | `["email", "firstName", "lastName"]` | Contact fields required before generating a checkout link |
| `generateCheckout.requirements.shippingAddress` | boolean | `false` | Whether a shipping address must be collected before checkout |

---

### `humanAssistance`

Human handoff and live agent escalation settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `humanAssistance.enabled` | boolean | `true` | Master toggle. When `false`, the entire human assistance flow exits immediately |
| `humanAssistance.inSchedule` | boolean | `true` | Whether the support team is within business hours. Used as the default when not overridden per-call |
| `humanAssistance.onlineAssistance` | boolean | `true` | Whether live agents are currently available. Used as the default when not overridden per-call |
| `humanAssistance.actions` | object[] | _(auto-generated)_ | Optional array of action definitions. When omitted, actions are auto-generated from `inSchedule` + `onlineAssistance` (see below) |

#### `humanAssistance.actions` — custom override

When provided, replaces the auto-generated action logic. Each item has:

```json
{ "action": "escalate | ticket | contact", "conditions": ["in_hours | out_of_hours | online | offline | always"] }
```

**`action` values:** `escalate` (transfer to live agent), `ticket` (create support ticket), `contact` (create/update contact record)

**`conditions` values:**

| Value | Meaning |
|-------|---------|
| `in_hours` | Currently within business schedule |
| `out_of_hours` | Currently outside business schedule |
| `online` | Live agents available (agent count > 0) |
| `offline` | No live agents available |
| `always` | Unconditional |

#### Auto-generated actions (when `humanAssistance.actions` is omitted)

| `inSchedule` | `onlineAssistance` | Generated actions |
|---|---|---|
| `true` | `true` | `escalate` on `in_hours` + `online`; `ticket` on `out_of_hours`; `ticket` on `offline` |
| `true` | `false` | `escalate` on `in_hours`; `ticket` on `out_of_hours` |
| `false` | `true` | `escalate` on `online`; `ticket` on `offline` |
| `false` | `false` | `escalate` always |

> A `contact` action with condition `always` is always prepended to every generated set.

---

### `orderStatus`

Order status lookup settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `orderStatus.includeInvoice` | boolean | `true` | Includes invoice data in the order status response (only when the `getOrderInvoice` provider is available) |
| `orderStatus.userIdPrompt` | string | `"User's DNI for check into their orders"` | Prompt shown when asking the user for their ID to look up orders |

---

### `productSearch`

Product search and catalog browsing settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `productSearch.enabled` | boolean | `true` | Enables product search |
| `productSearch.version` | string | `"v2"` | Product search API version (`"v1"` or `"v2"`) |
| `productSearch.askAddToCart` | boolean | `true` | Offers an add-to-cart option after showing search results (only when the `generateCheckout` provider is available) |
| `productSearch.showDiscovery` | boolean | `true` | Shows the category discovery flow when no specific product is found |
| `productSearch.filterByBranch` | boolean | `false` | Filters search results to only show products available at the user's selected branch |
| `productSearch.answerOnUnsatisfactory` | boolean | `true` | Generates a natural-language answer even when search results are unsatisfactory |
| `productSearch.recommendProduct` | boolean | `false` | Enables the product recommendation sub-flow alongside search |
| `productSearch.includeProductCharacteristicsInQuery` | boolean | `false` | Appends product characteristics (color, size, etc.) to the catalog search query |
| `productSearch.specificationFilter.skipValues` | string[] | `[]` | Specification values to exclude from product filters |

---

### `promotions`

Promotions flow settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `promotions.optionals` | string[] | `[]` | Optional data fields to include when fetching promotions |
| `promotions.validToday` | boolean | `false` | When `true`, filters promotions to only those valid today |
| `promotions.askForLocation` | boolean | `false` | When `true`, asks the user for their location to filter promotions by branch |
| `promotions.knowledgeFallback` | boolean | `false` | Falls back to the knowledge base when no promotions are found via the API |
| `promotions.useOptionsForLocation` | boolean | `false` | Shows location options (instead of free-text input) when asking for a location |
| `promotions.includePromotionsWithoutBranch` | boolean | `true` | Includes promotions that are not associated with any specific branch |

---

## AI & Orchestrator

---

### `awi`

Answer With Info (AI knowledge answering) settings.
This is the final moule which based on a set of knowledges (may be products, knowledges, branches, etc) generates an answer for the user based on them.


| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `awi.version` | number | `2` | Version of the AWI answering pipeline to use |

---

### `awk`

Answer With Knowledge (vector search answering) settings.
This will search in the knowledge and will generate an answer with `awi`.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `awk.generateQuery` | boolean | `false` | When `true`, generates an optimized search query from the user's message before performing the vector search |

---

### `gms`

Global Message System settings.
Responsible for managing user and bot messages and keeping the conversation history updated.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `gms.mergeMessages` | boolean | `true` | Merges consecutive messages of the same role/type into a single message before sending |

---

### `orchestratorFunctions`

Adds custom functions to the LLM orchestrator. Each entry must have a matching flow name in `config.flows`.

**Type:** `object[]`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | Unique function name. Must exactly match a key in `config.flows` |
| `description` | string | yes | Shown to the LLM to decide when to call this function. Must be non-empty and specific |
| `enabled` | boolean | no | Defaults to `true`. Set to `false` to hide the function from the LLM without removing it |
| `parameters` | object[] | no | Parameters the LLM extracts and passes to the flow |

Each entry in `parameters`:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Parameter name |
| `type` | string | Data type (`"string"`, `"number"`, etc.) |
| `required` | boolean | Whether the LLM must provide this parameter |
| `description` | string | Guidance for the LLM on what this parameter contains |

---

### `orchestratorOverwrites`

Modifies built-in orchestrator functions without fully replacing them. Use to disable, re-describe, or change parameters on any built-in function.

**Type:** `object` — keys are built-in function names, values are partial overrides.

```json
"orchestratorOverwrites": {
  "productSearch": {
    "enabled": false
  },
  "branches": {
    "description": "Use when the user asks about store locations or pickup points"
  }
}
```

Supported override fields: `description`, `enabled`, `parameters`.

---

### `splitTesting`

A/B version testing settings.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `splitTesting.enabled` | boolean | `false` | Enables the A/B version split test |
| `splitTesting.probability` | number | `50` | Percentage of conversations (0–100) that are assigned to version `v3`; the rest get `v2` |

---

# Custom Flows

The Dynamic Flow system lets you define multi-step conversation flows by composing reusable function building blocks. Flows are configured via `config.flows` and executed by the `[UF] Dynamic Flow` utility function.

## How it works

At runtime, `_flows()` merges the brain's internal `$flows` constant with `$config.flows`:

```
_flows() = mergeObj($flows, $config.flows)
```

When a flow is triggered, the system looks up the flow by name, then iterates through its steps sequentially. Each step calls one of the 20 built-in flow functions with optional parameters.

The built-in default flow (`flows.default`) is used as a fallback whenever a referenced flow name is not found:

```json
{ "flow": [{ "function": "awk" }] }
```

## Defining a custom flow

Add entries to `config.flows`. Each entry is a named flow with an array of steps:

```json
"config.flows": {
  "myFlowName": {
    "flow": [
      { "function": "<functionName>", "parameters": { ... } },
      { "function": "<functionName>" }
    ]
  }
}
```

Each step:
- `function` (required) — one of the 20 built-in function names listed below
- `parameters` (optional) — object passed directly to the function

## Calling a flow (`$dynamicFlow` parameters)

When invoking `[UF] Dynamic Flow`, pass a `$dynamicFlow` object:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `flow` | string \| object[] | — | Named flow key (string) to look up from `_flows()`, or an inline step array |
| `handleChangeSubject` | boolean | `false` | Whether to handle mid-flow topic changes |
| `cancelOnChangeSubject` | boolean | `true` | Cancels remaining steps when the user changes subject |

## Connecting a flow to the orchestrator

Defining a flow in `config.flows` only makes it available to call directly. For the LLM orchestrator to discover and invoke it automatically during conversation, you must also register it in `config.orchestratorFunctions`.

**Step 1 — Define the flow:**

```json
"flows": {
  "myFlowName": {
    "flow": [
      { "function": "awk", "parameters": { "query": "..." } }
    ]
  }
}
```

**Step 2 — Register it with the orchestrator:**

```json
"orchestratorFunctions": [
  {
    "name": "myFlowName",
    "description": "Triggered when the user asks about X. Describe clearly when the LLM should pick this.",
    "enabled": true,
    "parameters": [
      {
        "name": "myParam",
        "type": "string",
        "required": false,
        "description": "The value the user mentioned"
      }
    ]
  }
]
```

The `name` must exactly match the key in `config.flows`. The `description` is the only signal the LLM has to decide when to call this function — write it carefully.

**How it works internally:**

1. `composedOrchestratorFunctions` merges built-in functions with `config.orchestratorFunctions`
2. Custom entries are tagged `isCustom: true`
3. When the LLM selects a custom function, the orchestrator routes it to `[UF] Dynamic Flow` with `flow = function.name`
4. The Dynamic Flow looks up the name in `_flows()` (built-in `$flows` merged with `config.flows`) and executes the step sequence

---

# Flow Functions Reference

All 20 built-in functions available as steps inside a flow.

---

## `awk` — Answer With Knowledge

Vector knowledge search + AI-generated answer.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | user input | Search query sent to the knowledge base. Overridden by `knowledge.query` when set |
| `knowledge` | object | — | Fine-grained control over the knowledge fetch. All sub-fields are optional. Takes precedence over `query` when `knowledge.query` is set. Sub-fields: |
| `knowledge.key` | string | — | Fetch a specific knowledge entry by key, bypassing vector search |
| `knowledge.query` | string | — | Search query; priority order is `knowledge.query` > `query` > user input |
| `knowledge.amount` | number | `5` | Number of knowledge chunks to retrieve |
| `knowledge.chunks` | any | — | Chunk configuration passed to the knowledge API |
| `knowledge.categories` | string[] | — | Filter results to specific knowledge categories |
| `prompt` | string | — | Custom system prompt for the answering model |
| `model` | string | from config | Override the LLM model |
| `generateQuery` | boolean | from config | Generate an optimized query from the user message before searching |
| `negativePrompt` | string | — | Prompt guidance on what NOT to answer |
| `answerablePrompt` | string | — | Custom instructions for the answerability check |
| `answerOnUnsatisfactory` | boolean | `true` | Generate a response even when the knowledge doesn't satisfy the query |

---

## `awp` — Answer With Product

Shows a specific product and optionally offers to add it to cart.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `productId` | string | — | ID of the product to display |
| `query` | string | — | Fallback product search query |
| `categoryId` | string | — | Filter by category ID |
| `input` | string | user input | Override the user input used for context |
| `model` | string | from config | Override the LLM model |
| `showDiscovery` | boolean | `false` | Show discovery flow if no matching product is found |
| `offerAddToCart` | boolean | `true` | Offer an add-to-cart option after showing the product |
| `answerOnUnsatisfactory` | boolean | `true` | Answer even if the product result is unsatisfactory |

---

## `assistance` — Human Assistance

Triggers the human handoff flow (escalate to live agent, create ticket, or create contact).

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `inSchedule` | boolean | from config | Override business-hours flag |
| `onlineAssistance` | boolean | from config | Override online-agents flag |
| `actions` | object[] | from config | Override the full actions array (see `humanAssistance.actions` in config reference) |
| `reason` | string | — | User's problem statement passed to ticket/escalation |
| `ticketReason` | string | — | Reason used specifically for ticket creation |
| `appendUserInput` | boolean | `true` | Append the user's last message to the request |
| `handleChangeSubject` | boolean | `false` | Handle mid-flow subject changes |
| `additionalTicketFields` | object | — | Extra fields to include in the ticket payload |

---

## `ticket` — Create Ticket

Creates a support ticket, collecting required fields from the user if not pre-filled.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `reason` | string | — | Pre-filled ticket reason (skips asking the user) |
| `subreason` | string | — | Pre-filled ticket subreason |
| `description` | string | — | Pre-filled ticket description |
| `askForReason` | boolean | `true` if no reason | Whether to ask the user for a reason |
| `config` | string | `"default"` | Named ticket sub-config to use (e.g. `"fileAClaim"`) |
| `notification` | string | from config | Message shown to the user after the ticket is created |
| `allowOverride` | boolean | `true` | Allow the user to update an existing open ticket |
| `additionalData` | object | — | Extra data to attach to the ticket payload |
| `additionalFields` | object[] | from config | Extra fields to collect from the user |

---

## `contact` — Create Contact

Collects contact information and creates a CRM record.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `requirements` | string[] | from config | Override required contact fields |
| `additionalFields` | object[] | from config | Extra fields to collect |
| `useSnappyData` | boolean | from config | Pre-fill fields from existing Snappy user data |
| `appendUserInput` | boolean | `false` | Append user input to the contact record |

---

## `orderStatus` — Order Status

Looks up an order's current status and delivery information.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `orderNumber` | string | — | Pre-filled order number (skips asking the user) |
| `namespace` | string | — | Namespace scope for the order lookup |
| `userIdPrompt` | string | from config | Custom prompt when asking the user for their ID |

---

## `productDiscovery` — Product Discovery

Full product search and category discovery flow.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | — | Custom prompt appended to the product search context |
| `tolerance` | number | — | Search tolerance/threshold |
| `handleChangeSubject` | boolean | `false` | Handle mid-flow subject changes |

---

## `branches` — Branches

Searches for store branches by location or text query.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | string | from config | Override search mode (`"default"`, `"orchestrator"`, `"knowledge"`, `"full"`) |
| `location` / `query` | string | — | Location text or search query |
| `handleNoData` | boolean | `false` | Trigger a fallback if no branches are found |
| `handleChangeSubject` | boolean | `false` | Handle mid-flow subject changes |

---

## `promotions` — Promotions

Retrieves and displays store promotions.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `days` | string[] | — | Filter to promotions valid on specific days |
| `validToday` | boolean | from config | Only show promotions valid today |
| `optionals` | string[] | from config | Optional data fields to include |
| `handleNoData` | boolean | `false` | Trigger a fallback if no promotions are found |
| `askForLocation` | boolean | from config | Ask the user for a location to filter by branch |
| `knowledgeFallback` | boolean | from config | Fall back to the knowledge base if no promotions found via API |
| `useOptionsForLocation` | boolean | from config | Show location options instead of free-text input |
| `includePromotionsWithoutBranch` | boolean | from config | Include promotions not linked to a specific branch |

---

## `addProductToCart` — Add Product To Cart

Adds a specific product variant to the user's cart.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `cartProduct` | object | — | Product object to add (from catalog) |
| `quantity` | number | from config | Number of units to add |
| `productQuery` | string | — | Search query used to find the product if `cartProduct` is not provided |

---

## `accountPoints` — Account Points

Shows the user's loyalty/points balance and information.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `input` | string | user input | Override the user input used for context |
| `prompt` | string | — | Custom prompt for the points answer |
| `accountNumberPrompt` | string | — | Custom prompt when asking for the account number |

---

## `faq` — FAQ

Retrieves and answers a specific FAQ entry by key.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `value` | string | — | FAQ entry key to fetch |
| `input` | string | user input | Override the user input used for the answer |

---

## `menu` — Show Menu

Displays an IVR menu defined in the store's configuration.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `menu` | string | — | Menu key to look up from `$snappylabs.ivrData` |

---

## `sector` — Set Sector

Sets the conversation's active sector or department.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `value` | string | — | Sector identifier |

---

## `topic` — Set Topic

Sets the conversation's active topic label (tracked in metrics).

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `value` | string | — | Topic identifier |

---

## `survey` — Satisfaction Survey

Sends a satisfaction survey to the user.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `agentUuid` | string | — | UUID of the agent who handled the conversation |
| `satisfaction` | string | — | Pre-set result: `"positive"`, `"negative"`, or `"neutral"` |
| `userComment` | string | — | Pre-filled user comment |
| `askForMoreFeedback` | boolean | — | Whether to ask for an additional free-text comment |
| `customValue` | string | — | Custom satisfaction value |

---

## `ratingSurveyReply` — Rating Survey Reply

Submits a response to a rating survey, typically triggered by a postback action.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `agentUuid` | string | — | UUID of the rated agent |
| `value` | number | — | Rating value: positive (`> 0`), neutral (`0`), negative (`< 0`) |
| `customValue` | string | — | Custom rating label |
| `conversationUuid` | string | — | UUID of the conversation being rated |
| `integration` | string | — | Integration source identifier |
| `position` | number | — | Survey position index |

---

## `stockNotification` — Stock Notification

Subscribes the user to a stock availability alert for a product variant.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `productId` | string | — | ID of the product to watch |
| `productVariantId` | string | — | ID of the specific product variant |
| `productName` | string | — | Display name of the product |
| `productLink` | string | — | URL to the product page |
| `lang` | string | `$language` | Language for the notification message |

---

## `notify` — Notify

Sends a generated or static notification message to the user.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `notification` / `value` | string | — | Static message to send (bypasses AI generation when set) |
| `prompt` | string | — | Custom AI prompt to generate the notification text |
| `model` | string | — | Override the LLM model |
| `messages` | object[] | conversation history | Message context to use for generation |
| `temperature` | number | `0` | LLM temperature |
| `includeAssistancePrompt` | boolean | `true` | Include the assistance-offer instruction in the AI prompt |
| `offerAdditionalAssistance` | boolean | `true` | Instruct the AI to offer additional assistance at the end |

---

## `llm` — LLM (V3)

Invokes the V3 LLM handler directly. No parameters — driven entirely by the V3 orchestrator context.
