# Postbacks

Postbacks give information to the bot about options that are selected by the user. For each option set in any menu in the bot builder, there are two properties: **label** and **value**. **label** represents what the user sees in each clickable option box. **value** represents what the bot recieves as input (what we call the postback) when the user chooses said option. Postbacks can be evaluated using the **input.postback** variable.

## Client Menu Postbacks
These are some postbacks made specifically for clients to use in their menu configs to build their own conversational trees
 | Postback                   | Description                              |
 | -------------------------  | ---------------------------------------- |
 | action:menu:_menuUuid_     | Takes the user to another menu           |
 | action:knowledgeByKey:_knowledgeKey_ | Shows knowledge using a key |
 | action:createTicket:_ticketReason_ | Creates a ticket. _ticketReason_ is optional |
 | action:transferToHuman | Requests live human assistance |
 | action:getOrderStatus | Takes the user to the order status flow |
 | action:productSearch | Takes the user to the product catalog |
 | action:getBranches:_searchType_:_mainBranchUuid_ | Takes the user to the branches flow. The _searchType_ parameter can be 'askForLocation' or 'showAllBranches' and defines if the bot will ask the user for their location or show all branches directly. When using 'showAllBranches', a branch uuid can be provided as the second parameter to allow a specific branch to be shown first. |
 | action:survey | Takes the user to the satisfaction survey |
 | action:offerAdditionalAssistance | Offers the user additional assistance options |
 | action:setSector:_sector_ | Assigns the conversation to a sector |
 | action:setTopic:_topic_ | Appends a topic to the conversation topics and sets it as the conversation's current topic |
 | action:promotions:_todaysPromotions_ | Shows valid promotions (Promotions only valid today if the todaysPromotions parameter is set to true) |
 | action:faq:_faqKey_ | Shows the answer to an FAQ |
 | action:evaluateMessage | THIS MUST BE PLACED LAST! Continues with the evaluation of the given input text |

### Enabling Menu Postbacks in Notification Messages
Notification messages sent proactively to customers can support buttons, but due to gupshup limitations they do not support postback values. In order to work around this issue, there is a Notification Buttons package that uses notification contexts in order to process actions related to these buttons.

To set up your notification template to use actions in their buttons, a '**hasButtons**' context is required to indicate that your notification template has buttons with actions linked to them. You also need one context per each button, set in the following format: '**button:_button-label_:_actions_**' in such way that '**_button-label_**' corresponds to the text that goes in the button (must match exactly) and '**_actions_**' corresponds to the desired action that should take place when the user interacts with the button (all [Client Menu Postbacks](#client-menu-postbacks) are supported. Multiple actions can be used by separating each action with ';' and repeating the 'action:' string, e.g.: 'action:transferToHuman;action:survey')

For example, a notification template that uses two buttons should look like this (_note: the 'broadcast' context is unrelated to this example_):

![notification button contexts](/images/botbuilder/postbacks/notification-buttons-contexts.jpg)

_Disclaimer: in order to avoid any user inputs from potentially being flagged as button interactions incorrectly, these buttons will only be valid in the first interaction after the user receives their notification message. If the user types any other message or interacts with the bot in any other way, the buttons will no longer be valid as such._

>[!TIP]
>The same effect can be achieved without buttons using the contexts **hasPostNotificationAction** and **postNotificationAction:_actions_** In this case any actions set will be executed after the user replies to the notification, no matter what the user says in their message.

## Other Postbacks
These are some postbacks that we use in other flows internally
 ### Catalog
 | Postback        | Description   |
 | ---------------  | ------ |
 | selectCategory:_categoryId_     | Selects a category from the product catalog. Requires the conversation to have loaded categoriesData |
 | selectSubcategory:_categoryId_:_subCategoryId_          | Selects a subcategory from the product catalog. Requires the conversation to have loaded categoriesData |
 | filterBySpec:_specName_       | Sets a specification for the user to filter in the catalog (eg.: Filter by Color). Requires the conversation to have loaded specification data and a selected subcategory. _specName_ can be "none" to remove all applied filters |
 | filterBySpecValue:{"id":_specId_,"value":_specValue_} | Sets a value inside a specification to be filtered (eg.: Filter by Color: Blue). Requires the conversation to have a selected subcategory. _specValue_ can be "any/all" to remove filters for that _specId_. |
 | endSpecFiltering | Requires the conversation to have a selected product subcategory. Shows products from the selected subcategory with any filters previously applied |
 | searchOtherProducts | Restarts the catalog flow |
 | goBackToMainMenu | Returns to the main menu |
 | nothingElseThanks | Takes the user to the satisfaction survey |
 | removeProduct:_cartProductId_ | Removes a product from a cart by it´s cartProductId (requires a cart) |
 | startRemoveProduct | Delivers user to Remove Product flow | 
 | modifyProduct:_cartProductId_ | Modifies quantity or variants of a selected product by it´s  cartProductId
 | startModifyProduct | Delivers user to Modify Product flow |
 | endPurchase |  Delivers user to End Purchase flow |
 | addProduct | Delivers user to Product Search flow |
