# About
This document shows and explains what data is sent to BotBuilders via the conversation context in order to know know to use it.

## Custom Contexts
Custom Contexts are contexts that can be applied by clients in the **behaviour** tab of the bot configuration in Labs. They can be used to alter the bot's behaviour in certain flows without necessarily using the bot builder to edit said flows, with the requirement that the flow has been built with the custom contexts in mind.

There are currently no supported Custom Contexts. Previously supported custom contexts were replaced by the use of custom [configs](#Config)

## Custom JSON Contexts
Custom JSON Contexts are very similar to Custom Contexts in the sense that they can be set by the client. However, Custom JSON Contexts are in the format of a JSON Object that can be edited freely by the client, to leverage different JSON data types. The bot support the following Custom JSON Contexts:

### Config
The config object is used to modify the bot's behaviour in different flows. Clients can override the config using Custom JSON Contexts. The Custom JSON Contexts do not need to have the complete config context, instead clients can just override the values that they want to change. For example, if the client wants to change the branches flow requirements, they only need to copy that part in their Custom JSON Contexts, not the whole config object.

This is the default config JSON (when not overwritten by a custom Config):
```
"config": {
	"flags": {
		"limitedFacebookBot": false,
		"limitedWhatsappBot": false,
		"limitedInstagramBot": false,
		"forcedTermsAndConditions": false
	},
	"branches": {
		"enabled": true,
		"requirements": [
			"location"
		]
	},
	"createTicket": {
		"default": {
			"optionals": [
				"files",
				"comment"
			],
			"requirements": [
				"ticketReason",
				"ticketSubreason"
			],
			"MinFilesAmount": ""
		},
		"enabled": true
	},
	"createContact": {
		"enabled": true,
		"requirements": [
			"firstName",
			"email",
			"phone"
		]
	},
	"productSearch": {
		"enabled": true,
		"specificationFilter": {
			"skipValues": []
		}
	},
	"humanAssistance": {
		"default": {
			"requirements": []
		},
		"enabled": true,
		"fileAClaim": {
			"requirements": []
		}
	},
	"generateCheckout": {
		"enabled": true,
		"requirements": [
			"firstName",
			"lastName",
			"email"
		]
	}
}
```

| Config Name | Default Values | Possible Values |
| ------------- | ------------- | ------------- |
| branches -> requirements | location | location |
| createContact -> requirements | firstName, email, phoneNumber | fistName, lastName, email, phone, documentNumber |
| createTicket -> default -> requirements | ticketReason, ticketSubreason, comment | ticketReason, ticketSubreason, comment, files, orderN, any additional data that the bot should ask for |
| createTicket -> default -> optionals | files | comment, files |
| createTicket -> default -> filesAmmount | null | Any number |
| flags | "forcedTermsAndConditions": false, "limitedInstagramBot": false, "limitedWhatsappBot": false, "limitedFacebookBot": false | Personalized values are allowed |
| generateCheckout -> requirements | firstName, lastName, email | fistName, lastName, email |
| humanAssistance -> default -> requirements |  | contact, ticket |
| productSearch -> specificationFilter -> skipValues |  | Personalized values are allowed |

The *flags* object simply contains flags that enable/disable behaviours not specific to any flows (similar to older custom contexts).

Inside both *createTicket* and *humanAssistance* there is a "default" object containing each flow's default configuration, but additional objects can be introduced for different intents, meaning the bot will use a different configuration when creating a ticket or requesting human assistance for that specific intent. In the case of *humanAssistance*, having an intent present in the config will trigger a human assistance request when that intent is recognized, after the bot gives a response.

If we put something in [createTicket -> default -> optionals] it must not be inside [createTicket -> default -> requirements] and viceversa.

Here's an example for a custom createTicket config for the intent "fileAClaim":
```
{
    "createTicket": {
      "enabled": true,
      "default": {
        "optionals": [
          "files"
        ],
        "requirements": [
          "ticketReason",
          "ticketSubreason",
          "comment"
        ],
        "MinFilesAmount": ""
      },
      "fileAClaim": {
        "requirements": [
          "ticketReason",
          "file",
          "example1",
          "example2"
        ]
      }

    }
}

```

"example1" and "example2" would need to be specified in the "texts" tab with its own translation (translation that has to be a request for the user to give that information). As in the following image:

<img width="1428" alt="Screenshot 2024-08-29 at 14 33 32" src="https://github.com/user-attachments/assets/b3c9ab99-2dde-47d7-9d97-0cce94ab8f38">

### Custom Topics Data
Allows the client to map different topics to new ticket reasons and subreasons (subreasons optional). The Custom Topics Data can override an existing topic-reason map or assign reasons and subreasons to custom topics set by the client through menus.
Used in the following format:
```
{
  "customTopicsData": {
    "desiredTopic": {
        "ticketReason": "desiredReason",
        "ticketSubreason": "desiredSubreason"
      }
  }
}
```

### Action By Message
Allows the client to map a specific message(s) to [menu actions](https://github.com/SnappyCommerce/PublicDocs/blob/main/botbuilder/Postbacks.md#client-menu-postbacks). This way, when the bot recieves a set message, it executes the defined actions. Multiple actions can be used by separating each action with '**;**' and repeating the '**action:**' string, e.g.: '_action:transferToHuman;action:survey_'. Used ideally with whatsapp links that come with preset messages.
> [!CAUTION]
> If the set message is too generic, there is a risk that a user will type in the message, which could result in strange bot behaviour

Used in the following format:
```
{
    "actionByMessage": [
            {
                "message": "message",
                "actions": "actions string"
            }
        ]
}
```

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
