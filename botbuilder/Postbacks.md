# Postbacks

Postbacks give information to the bot about options that are selected by the user.

### Client Menu Postbacks
Here we'll document postbacks that clients can use in their menu configs to build their own conversational trees
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

### Other Postbacks
These are some postbacks that we use in other flows internally
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
 