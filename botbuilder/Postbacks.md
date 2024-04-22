# Postbacks

Postbacks give information to the bot about options that are selected by the user. For each option set in any menu in the bot builder, there are two properties: **label** and **value**. **label** represents what the user sees in each clickable option box. **value** represents what the bot recieves as input (what we call the postback) when the user chooses said option. Postbacks can be evaluated using the **input.postback** variable.

## Client Menu Postbacks
These are some postbacks made specifically for clients to use in their menu configs to build their own conversational trees
 | Postback                   | Description                              |
 | -------------------------  | ---------------------------------------- |
 | action:menu:_menuUuid_     | Takes the user to another menu           |
 | action:knowledge:_knowledgeQuery_                    | Shows knowledge using a set query                                   |
 | action:knowledgeByKey:_knowledgeKey_                 | Shows knowledge using a key                                   |
 | action:fetchKnowledgeAndCreateTicket:_knowledgeQuery_:_ticketReason_ | Shows knowledge using a set query and then jumps to create a ticket. _ticketReason_ is optional |
 | action:fetchKnowledgeByKeyAndCreateTicket:_knowledgeKey_:_ticketReason_ | Shows knowledge using a key and then jumps to create a ticket. _ticketReason_ is optional |
 | action:createTicket:_ticketReason_ | Creates a ticket. _ticketReason_ is optional |
 | action:transferToHuman | Requests live human assistance |
 | action:getOrderStatus | Takes the user to the order status flow |
 | action:productSearch | Takes the user to the product catalog |
 | action:getBranches | Takes the user to the branches flow |
 | action:survey | Takes the user to the satisfaction survey |
 | action:offerAdditionalAssistance | Offers the user additional assistance options |
 | action:setSector:_sector_ | Assigns the conversation to a sector |

### Enabling Menu Postbacks in Notification Messages
Notification messages sent proactively to customers can support buttons, but due to gupshup limitations they do not support postback values. In order to work around this issue, there is a Notification Buttons package that uses notification contexts in order to process actions related to these buttons.

To set up your notification template to use actions in their buttons, a '**hasButtons**' context is required to indicate that your notification template has buttons with actions linked to them. You also need one context per each button, set in the following format: '**button:_button-label_:_actions_**' in such way that '**_button-label_**' corresponds to the text that goes in the button (must match exactly) and '**_actions_**' corresponds to the desired action that should take place when the user interacts with the button (all [Client Menu Postbacks](#client-menu-postbacks) are supported. Multiple actions can be used by separating each action with ';')

For example, a notification template that uses two buttons should look like this (_note: the 'broadcast' context is unrelated to this example_):

![notification button contexts](/images/botbuilder/postbacks/notification-buttons-contexts.jpg)

_Disclaimer: in order to avoid any user inputs from potentially being flagged as button interactions incorrectly, these buttons will only be valid in the first interaction after the user receives their notification message. If the user types any other message or interacts with the bot in any other way, the buttons will no longer be valid as such._

## Other Postbacks
These are some postbacks that we use in other flows internally
 ### Catalog
 | Postback        | Description   |
 | ---------------  | ------ |
 | selectCategory:_categoryId_     | Selects a category from the product catalog. Requires the conversation to have loaded categoriesData |
 | selectSubcategory:_categoryId_:_subCategoryId_          | Selects a subcategory from the product catalog. Requires the conversation to have loaded categoriesData |
 | filterBySpec:_specName_       | Sets a specification for the user to filter in the catalog (eg.: Filter by Color). Requires the conversation to have loaded specification data and a selected subcategory. _specName_ can be "none" to remove all applied filters |
 | filterBySpecValue:{"id":_specId_,"value":_specValue_} | Sets a value inside a specification to be filtered (eg.: Filter by Color: Blue). Requires the conversation to have a selected subcategory. _specValue_ can be "any/all" to remove filters for that _specId_. |
 | specOptionsNextPage | Shows the next page within the specification filtering menu |
 | specOptionsLastPage | Shows the previous page within the specification filtering menu |
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
