# About
This document shows and explains what data is sent to BotBuilders via the conversation context in order to know know to use it.

## Custom Contexts
Custom Contexts are contexts that can be applied by clients in the **behaviour** tab of the bot configuration in Labs. They can be used to alter the bot's behaviour in certain flows without necessarily using the bot builder to edit said flows, with the requirement that the flow has been built with the custom contexts in mind.
| Value | Description |
| ------------- | ------------- |
| noTicketCreation | Disables the ticket creation and instead responds with a text when needed (translation path: botFunctions.createTicket.ticketCreationDisabled) |
| skipSpecificationFilter:_filterName_ | Stops any specification filter from showing up among the filters offered to the user (e.g.: skipSpecificationFilter:Color) |
| contacts.requiresEmail | Forces contacts to have an email before creating a ticket |
| contacts.skipPhoneNumber | Skips the question for a user's phone number when creating a contact |
| disableWhatsappBot | Limits all bot interactions in whatsapp to requesting human assistance |
| disableInstagramBot | Limits all bot interactions in instagram to requesting human assistance |
| disableFacebookBot | Limits all bot interactions in facebook to requesting human assistance |
| forceTermsAndConditions | Requests user to accept terms and conditions before continuing conversation |

## Main Context
These are all the contexts that the bot receives from labs at the start of or during a conversation. These can all be evaluated and used by the bot together with any contexts set by the bot itself in the bot builder.
| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| [snappylabs](#snappy-labs) | Snappylabs Information | Object |
| botName | Bot's Name | String |
| botUuid | Bot's Uuid | String |
| conversationUuid | Conversation's Uuid | String |
| storeUuid | Store's Uuid | String |
| channel | Conversation's channel (Possible values: `websocket`, `whatsapp`, `instagram`, `facebook`) | String |
| remoteUrl | The URL the user is writing from (if channel is websocket) | String |
| [storeInfo](#storeInfo) | Store's info | Object |
| [customContexts](#custom-contexts) | Holds all custom contexts set by the clients in labs as booleans | Object |
| [lastMessage](#lastMessage) | Information about that last message sent by the user | Object |
| [notificationMetadata](#notification-metadata) | Information about any notification/broadcast sent | Object |
| internalMessageResponse | Contains the response from any received [internal message](InternalMessages.md) | Object |
| [snappyData](#snappyData) | Contains user contact information received from external sources | Object |
| username | The user's instagram handle (if the channel is instagram) | String |
| phoneNumber | The user's phone number (if the channel is whatsapp). | String |
| firstName | The user's first name (if the channel is whatsapp, instagram or facebook | String |
| facebookId | The user's Facebook User ID | String |
| instagramId | The user's Instagram User ID | String |


## Snappy Labs
The following data is inside: `context.snappylabs` or `$snappylabs`

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| [integrations](#integrations) | Bot's enabled integrations | Object |
| [promotions](#promotions) | Bot's promotions configuration | Object |
| [branches](#branches) | Bot store's branches | Object |
| [tickets](#tickets) | Bot's ticket configuration | Object |
| [productSearch](#product-search) | Results from a product search | Object |
| [orderStatus](#order-status) | Results from an order status query | Object |
| language | The bot's configured language, in two letter [ISO 639 language codes](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes) | String |
| [providers](#providers) | Bot's integration providers | Object |
| checkoutLink | The store's checkout link | String |
| isAssistanceOnline | true during the store's attention schedule. Updates with every message | Boolean |
| [ivrData](#ivrData) | Bot's menus information | Object |


### Integrations
The integrations object gives the bot information about what integrations are currently enabled

In this sub-object (**$snappylabs.integrations**) each key represents an integration and the value tells if enabled.

> **Example:** *if $snappylabs.integration.vtex = true* 

> **Important:** *This table is automatically filled, meaning that for each new integration a new key will appear. This example may be missing some key names.*

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| [Integration Name] | Is **[Integration Name]** enabled? | Boolean |
| castillo | Is **Castillo** enabled? | Boolean |
| forus | Is **Forus** enabled? | Boolean |
| freshdesk | Is **Freshdesk** enabled? | Boolean |
| glamit | Is **Glamit** enabled? | Boolean |
| googleAnalytics | Is **GoogleAnalytics** enabled? | Boolean |
| markova | Is **Markova** enabled? | Boolean |
| vtex | Is **VTEX** enabled? | Boolean |
| woowup | Is **Woowup** enabled? | Boolean |
| zendesk | Is **Zendesk** enabled? | Boolean |

### Promotions
The promotions sub-object tells the bot information about the bot's store promotions.

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| hasPromotions | Does this bot's store has currently active promotions | Boolean |

> **hasPromotions:** Will return **true/false** if there are **any** currently active promotions and is updated for each message sent to the bot.

### Branches

This object will tell the bot information about a store's branches
| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| hasBranches | Tells us if the client has any branches location configured in labs | Boolean |
| quantity | The amount of branches loaded in labs | Number |

### Tickets

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| ticketConfirmationEmail | Tells us if the client has their config set to require ticket validation via email | Boolean |

### Product Search
After a product search is conducted using the [product search output](Messages.md#product-search-message), the bot will recieve an additional internal message with the text `#internal/product-search` and the following contexts (inside $snappylabs.productSearch)

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| showedProducts | true if products were successfully shown | Boolean |
| queryRealized | The query used to search for products | Object |

### Order Status
Similarly to product search, after an order status query is conducted using the [order status output](Messages.md#order-status-message), the bot will recieve an additional internal message carrying the following contexts (inside $snappylabs.orderStatus)

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| statusCode | The code of the order's status (e.g.: `delivered`) | String |
| estimatedDate | The estimated date for the order's delivery in [yyyy-MM-dd Unicode Technical Standard #35 format](https://www.unicode.org/reports/tr35/tr35-dates.html#Date_Field_Symbol_Table) | String |
| hasTrackingNumber | true if a tracking number is available for the order | Boolean |
| trackingNumber | The order's tracking number | String |
| orderUrl | The order's tracking URL | String |
| orderId | The order's ID number | String |
| date | A timestamp of when the order was placed | String |

### Providers
Contains information about the providers for the bot's different functions. Each item is an array containing all of the active providers for said item. e.g.: If the store has two order status providers, Andreani and VTEX, then $snappylabs.providers.orderStatus will be `["Andreani","VTEX"]`. If it has none, then it will be `[]`

These are the possible items a store might have providers for:
- getContact
- getCheckout
- orderStatus
- chatTransfer
- syncContacts
- productSearch
- getOrdersById
- getCheckoutCart
- getOrderInvoice
- generateCheckout
- productCategories
- updateCheckoutCart
- categorySpecifications
- subscribeToStockNotifications

### ivrData
Contains information about the store's configured menus. Each property of the object represents a menu, with its uuid as the key for the object. This is the information that each menu holds:

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| uuid | The menu's uuid | String |
| title | The menu's title (what will be shown as the header above the options) | String |
| options | An array of all the options in the menu with their label, value and position. For more information about options and how they behave, see [Postbacks](Postbacks.md) | Array |
| createdAt | A timestamp of when the menu was created | String |
| isMainMenu | true if this is the main menu (main menus are shown at the start of conversations) | Boolean |

## storeInfo
The following data is inside $storeInfo

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| name | The store's name as it is set in labs | String |
| url | The store's url as it is set in labs | String |
| mail | The store's email address as it is set in labs | String |

## lastMessage
The following data is inside $lastMessage

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| isFile | true if the last message sent was a file, false if not | Boolean |
| fileType | The type of file sent by the user (e.g.: `image/jpeg`, `video/mp4`) | String |
| meta | Tells us if the last message was a social media interaction of some sort and of what type (e.g.: `instagram_post_share`, `instagram_story_mention`, `instagram_story_reply`, `shared_meta_product`) | String |
| isProduct | true if the last message sent was a product (from Facebook, for example) | Boolean |
| product | Information from the product sent | Object |


## Notification Metadata
The following data is inside $notificationMetadata These contexts will only be present in the conversation if the user received a notification (also called broadcasts).

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| uuid | The notification's uuid | String |
| userId | Uuid of the contact the notification was sent to | String |
| actions | Lists the actions taken along with the notification. Each action is an object, containing the `action` value which determines if the action was `assignToSector` or `assignToUser`, and `sector` which determines the sector the conversation was assigned to, or `userUuid` which determines the Uuid of the labs user the conversation was assigned to. e.g.: `[{"action":"assignToSector","sector":"sales"},{"action":"assignToUser","userUuid":"exampleUuid"}]` | Array |
| contexts | Similar to custom contexts, holds all contexts set for a notification by the clients in labs as booleans | Object |
| timestamp | The timestamp of when the notification was received | String |


## snappyData
The following data is inside $snappyData This is contact information received from external sources such as the user's VTEX account information, for example.

| Key Name | Description | Type |
| ------------- | ------------- | ----- |
| loginEmail | The user's email address | String |
| loginFirstName | The user's first name | String |
| loginLastName | The user's last name | String |
| loginPhone | The user's phone number | String |
| loginDocumentType | The user's document type (e.g.: `DNI`) | String |
| loginDni | The user's document number | String |
| email | The user's email address | String |
| orderNumber | The user's last order number | String |
| firstName | The user's first name | String |
| phoneNumber | The user's phone number | String |
| isSubscriber | true if the user is subscribed to the newsletter | Boolean |
