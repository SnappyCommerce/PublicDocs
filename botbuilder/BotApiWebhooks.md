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
 | metadata	    | Array  | false    | Array of metadata values                      |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |
 | asyncIntegration | Boolean| If true will  run asynchronous the integration |

**Note:** webhook call should have at least a mail, phone, facebookId or instagramId
**Note:** If conversationUuid is passed it will set the retrieved contact as the contact of the conversation
**Note:** Metadata is an array of metadata values like: {key: 'vtex_id', type: 'string', value: '2kl312-312-dfdfs-e12123'}

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
 | metadata	    | Array  | false    | Array of metadata values                      |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |

**Note:** If conversationUuid is passed it will set the retrieved contact as the contact of the conversation
**Note:** Metadata is an array of metadata values like: {key: 'vtex_id', type: 'string', value: '2kl312-312-dfdfs-e12123'}

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
 ### endConversation 
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "endConversation"                               |
 | botUuid          | String | true     | botUuid that is saved in the context          |
 | conversationUuid | String | false    | conversationUuid that is saved in the context |

**Note:** Use this as an output instead of webhook to prevent your bot builder from sending a message after conversation ended

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
 ---
 ### getOrderInvoice
 #### Parameters
 | Parameter        | Type   | Description                            |
 | ---------------  | ------ | ---------------------------------------|
 | functionName     | String | "getOrderInvoice"                     |
 | botUuid          | String | botUuid that is saved in the context   |
 | orderId          | String | orderId of the order                   |

 #### Result Example
 ```json
	{
		"invoiceUrl": "www.google.com"
	}
 ```
 ---
 ### fetchProducts
 #### Parameters
 | Parameter        | Type   | Description                            |
 | ---------------  | ------ | ---------------------------------------|
 | functionName     | String | "fetchProducts"                        |
 | botUuid          | String | botUuid that is saved in the context   |
 | query            | String | Search products by key words           |
 | categoryId       | String | Search products by category            |
 | subCategoryId    | String | Search products by sub category        |

 #### Result Example
 ```json
{
  "products": [
    {
      "id": "1",
      "title": "Test product",
      "description": "Test description",
      "link": "https://www.test.com.ar/1/p",
      "products": [
        {
          "id": "235",
          "price": 4035.25,
          "variants": [
            "17335"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        },
        {
          "id": "458",
          "price": 3750,
          "variants": [
            "17331"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        },
        {
          "id": "450",
          "price": 4035.25,
          "variants": [
            "17336"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        },
        {
          "id": "115",
          "price": 3750,
          "variants": [
            "17332"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        },
        {
          "id": "114",
          "price": 4035.25,
          "variants": [
            "17333"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        },
        {
          "id": "848",
          "price": 3750,
          "variants": [
            "17334"
          ],
          "discount": 40,
          "images": [
            {
              "url": "https://www.test.com.ar/1/image1.png"
            }
          ],
          "isVisible": true
        }
      ],
      "variants": [
        {
          "id": "17331",
          "type": "Color",
          "value": "BERRY"
        },
        {
          "id": "17334",
          "type": "Color",
          "value": "BORGONA"
        },
        {
          "id": "17332",
          "type": "Color",
          "value": "DEEP ROSE"
        },
        {
          "id": "17335",
          "type": "Color",
          "value": "NUDE"
        },
        {
          "id": "17336",
          "type": "Color",
          "value": "RED"
        },
        {
          "id": "17333",
          "type": "Color",
          "value": "VIOLETA"
        }
      ]
    }
  ]
}
 ```
---

### getOrder
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "gerOrder"                                    |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | orderId          | String |   true   | orderId of the order                          |
 | validation       | String |   false  | validation for that oreder                    |

 #### Result Example
 ```json
{
	"order": {
		"statusCode":"done",
		"estimatedDate":"2023-09-28T13:57:50-03:00",
		"trackingNumber":"xxxxxxxxxx",
		"orderUrl": "https://www.google.com"
	}
}
 ```
 ---
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
