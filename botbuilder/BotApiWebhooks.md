# Webhook functions

### getTicket
 #### Parameters
 
 | Parameter        | Type   | Description                           |
 | ---------------  | ------ | ------------------------------------- |
 | functionName     | String | "getTicket"                           |
 | botUuid          | String | botUuid that is saved in the context  |
 | ticketUuid       | String | Ticket Uuid                           |

 #### Result Example
 ```json
	{
		"ticket": {
			"id": "44",
			"uuid": "c1b41f1c-4853-4db6-8384-3e26ab86b0a4",
			"title": "Ticket facu3 - ",
			"description": " - ",
			"sectorId": "1",
			"reasonId": "7",
			"subreasonId": null,
			"statusId": "1",
			"priority": "3",
			"storeId": "3",
			"contact": {
				"uuid": "9faf2caa-ef38-4d10-a2c8-772e4207ee08",
				"firstName": "facu3",
				"lastName": null,
				"phone": null,
				"email": "tes@test.com",
				"country": null
			},
			"assignedTo": null,
			"createdBy": null,
			"createdAt": "2021-03-03T18:42:48.540Z",
			"updatedAt": null,
			"conversationUuid": "2602fcd0-7c50-11eb-9ea9-eb620888afcb",
			"status": {
				"id": "1",
				"key": "open",
				"name": "Open",
				"color": "#f5f5f5",
				"storeId": null,
				"isDefault": true
			}
		}
	}
 ```
 ---
### createTicket
 #### Parameters
 | Parameter        | Type   | Description                                    |
 | ---------------  | ------ | ---------------------------------------------- |
 | functionName     | String | "createTicket"                                 |
 | botUuid          | String | botUuid that is saved in the context           |
 | conversationUuid | String | conversationUuid that is saved in the context  |
 | ticket           | Object | ticket object*                                 |
 | asyncIntegration | Boolean| If true will  run asynchronous the integration |

##### *Ticket Object:
 | Propertie       | type   | Description                                      |
 | --------------- | ------ | -------------------------------------------------|
 | title           | String | Ticket title                                     |
 | description     | String | ticket description                               |
 | priority        | Number | Number from 1 to 5. 1 beeing the lowest priority |
 | status          | String | valid status key (It has to be in labs)          |
 | ticketReason    | String | valid reason key (IT has to be in labs)          |

 #### Result example
 ```json
	{
		"ticket": {
			"id": "44",
			"uuid": "c1b41f1c-4853-4db6-8384-3e26ab86b0a4",
			"title": "Nuevo Ticket facu3 - ",
			"description": " - ",
			"sectorId": "1",
			"reasonId": "7",
			"subreasonId": null,
			"statusId": "1",
			"priority": "3",
			"storeId": "3",
			"assignedTo": null,
			"createdBy": null,
			"createdAt": "2021-03-03T18:42:48.540Z",
			"updatedAt": null,
			"conversationUuid": "2602fcd0-7c50-  11eb-9ea9-eb620888afcb",
			"status": {
				"id": "1",
				"key": "open",
				"name": "Open",
				"color": "#f5f5f5",
				"storeId": null,
				"isDefault": true
			}
		}
	}
 ```
 ___
 ### retrieveContact
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "retrieveContact"                             |
 | botUuid          | String | true     | botUuid that is saved in the context          |
 | email            | String | false    | email of the contact to be retrieved          |
 | phone            | String | false    | phone of the contact to be retrieved          |
 | contactUuid      | String | false    | retrieve a specific contact                   |
 | facebookId       | String | false    | facebookId (It is an internal value)          |
 | instagramId      | String | false    | instagramId (It is an internal value)         |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |

**Note:** webhook call should have at least a mail or a phone
**Note:** If conversationUuid is passed it will set the retrieved contact as the contact of the conversation

 #### Result Example
 ``` json
 {
	"contact": {
		"firstName":  "Test",
		"lastName":  "test",
		"email":  "test@test.com",
		"phone":  "123465789",
		"country":  null,
		"isSubscriber":  false,
		"verificated":  true,
		"censoredPhone":  "*****5789"
	}
}
 ```
 ---
 ### createContact 
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "createContact"                               |
 | botUuid          | String | true     | botUuid that is saved in the context          |
 | email            | String | false    | email of the new contact                      |
 | phone            | String | false    | phone of the new contact                      |
 | facebookId       | String | false    | facebookId (It is an internal value)          |
 | instagramId      | String | false    | instagramId (It is an internal value)         |
 | firstName        | String | false    | firstName of the new contact                  |
 | lastName         | String | false    | lastName of the new contact                   |
 | country          | String | false    | country of the new contact                    |
 | isSubscriber     | Boolean| false    | wheter the new contact is subscribed or not   |
 | customContactId  | String | false    | External custom id                            |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |
 | asyncIntegration | Boolean| If true will  run asynchronous the integration |

**Note:** webhook call should have at least a mail, phone, facebookId or instagramId
**Note:** If conversationUuid is passed it will set the retrieved contact as the contact of the conversation

#### Result Example
```json
{
	"contact": {
		"id":"305",
		"uuid":"17bda479-dcf6-41cb-8a07-68957766d8be",
		"firstName":"Test",
		"lastName":"Webhook",
		"email":"test@test.com",
		"phone":"123456789",
		"country":"Argentina",
		"isSubscriber":true,
		"storeId":"1",
		"verificated":false,
		"isDeleted":false,
		"createdAt":"2021-03-22T15:38:02.745Z",
		"updatedAt":null
	}
}
```
---
 ### updateContact 
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "updateContact"                               |
 | botUuid          | String | true     | botUuid that is saved in the context          |
 | contactId        | String | true     | contac id of the contact to update            |
 | email            | String | false    | new email of the contact                      |
 | phone            | String | false    | new phone of the contact                      |
 | facebookId       | String | false    | facebookId (It is an internal value)          |
 | instagramId      | String | false    | instagramId (It is an internal value)         |
 | firstName        | String | false    | new firstName of the contact                  |
 | lastName         | String | false    | new lastName of the contact                   |
 | country          | String | false    | new country of the contact                    |
 | isSubscriber     | Boolean| false    | wheter the new contact is subscribed or not   |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |

**Note:** If conversationUuid is passed it will set the retrieved contact as the contact of the conversation

```json
{
	"contact": {
		"id":"238",
		"uuid":"e7e2d385-c989-4509-8ede-6c05bd764c0b",
		"firstName":"Test",
		"lastName":"Test",
		"email":"test@test.com",
		"phone":"987654321",
		"country":"Argentina",
		"isSubscriber":true,
		"storeId":"1",
		"verificated":false,
		"isDeleted":false,
		"createdAt":"2021-01-21T14:09:32.126Z",
		"updatedAt":"2021-03-22T15:41:23.606Z"
	}
}
```
 ---
 ### onlineAssistants
 #### Parameters
 | Parameter        | Type   | Description                            |
 | ---------------  | ------ | ---------------------------------------|
 | functionName     | String | "onlineAssistants"                     |
 | botUuid          | String | botUuid that is saved in the context   |

 #### Result Example
 ```json
	{
		"onlineAssistants": 1
	}
 ```
 # Webhook Output
 Now you can call a webhook as an output
 ```js
 {
 	"type": "webhook",
	"value": {
		// Webhook function payload
	}
 }
```
For the property `value` you need to provide the payload of any of the functions from above.

This will always respond with an output called `#internal/output-response`. This response will add two properties to the context.
- `outputResponse` will have the response. If there is an error will return null.
- `outputResponseError` if there is an error will return the error data otherwise will return null.