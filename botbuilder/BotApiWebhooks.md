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
 | Property        | type   | Description                                      |
 | --------------- | ------ | -------------------------------------------------|
 | title           | String | Ticket title                                     |
 | description     | String | ticket description                               |
 | priority        | Number | Number from 1 to 5. 1 beeing the lowest priority |
 | status          | String | valid status key (It has to be in labs)          |
 | ticketReason    | String | valid reason key (IT has to be in labs)          |
 | ticketSubreason | String | valid subreason key (IT has to be in labs)       |

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
### updateTicket
 #### Parameters
 | Parameter        | Type   | Description                                       |
 | ---------------  | ------ | ------------------------------------------------- |
 | functionName     | String | "updateTicket"                                    |
 | ticket           | Object | ticket object*                                    |
 | ticketUuid       | String | ticket Uuid (ticketUuid or ticketFid is required) |
 | ticketFid        | String | ticket Fid (ticketUuid or ticketFid is required)  |

##### *Ticket Object:
 | Property        | type   | Description                                      |
 | --------------- | ------ | -------------------------------------------------|
 | title           | String | Ticket title                                     |
 | description     | String | ticket description                               |
 | priority        | Number | Number from 1 to 5. 1 beeing the lowest priority |
 | status          | String | valid status key (It has to be in labs)          |
 | ticketReason    | String | valid reason key (IT has to be in labs)          |
 | ticketSubreason | String | valid subreason key (IT has to be in labs)       |

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
 ### fetchShippingAddresses
 | Parameter        | Type   | Required | Description                           	    |
 | ---------------  | ------ | -------- | --------------------------------------------- |
 | functionName     | String | true     |"fetchShippingAddresses"              		    |
 | botUuid          | String | true     | botUuid that is saved in the context    		|
 | conversationUuid | String | true     | conversationUuid that is saved in the context |

**Notes:** 

-Conversation must have a contact linked

 ---
 ### updateContactAddress 
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String | true     | "updateContactAddress"                        |
 | botUuid          | String | true     | botUuid that is saved in the context          |
 | conversationUuid | String | true     | conversationUuid that is saved in the context |
 | addressId		| String | false	| addressId of the address you want to modify 	|
 | addressUuid		| String | false	| addressUuid of the address you want to modify |
 | address			| Object | true 	| address object							 	|

**Notes:** 

-Conversation must have a contact linked

-Leaving both addressId and addressUuid empty will create a new address for the conversation's contact

 #### address object 
```json
{
	"country": "Argentina",
	"province": "Buenos Aires",
	"city": "Luján",
	"street": "Calle Falsa",
	"number": "4321",
	"floor": "floor 3, room 4", // optional
	"postalCode": "1234",
	"notes": "The building is pale blue, you'll find it easily", // optional
}
```

 #### Result Example
```json
{
	"contactAddress": {
		"id": "15",
		"uuid": "c88d6536-cc5f-43cd-9172-a14ac2874f9e",
		"contactId": "38520",
		"country": "Argentina",
		"province": "Buenos Aires",
		"city": "Luján",
		"street": "Calle Falsa",
		"number": "4321",
		"floor": null,
		"postalCode": "1234",
		"notes": "The building is pale blue, you'll find it easily",
		"createdAt": "2025-01-16T16:47:27.720Z",
		"updatedAt": "2025-01-16T16:47:27.720Z"
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
 | Parameter           | Type   | Description                                     |
 | ---------------     | ------ | ------------------------------------------------|
 | functionName        | String | "fetchProducts"                                 |
 | botUuid             | String | botUuid that is saved in the context            |
 | query               | String | Search products by key words                    |
 | categoryId          | String | Search products by category            	  |
 | subCategoryId       | String | Search products by sub category                 |
 | selectedCategory    | String | Search products by category (recommended)       |

 #### Result Example
 ```json
{
  "products": [
    {
      "id": "1",
      "title": "Test product",
      "description": "Test description",
      "link": "https://www.test.com.ar/1/p",
	 "specifications": {
	    "Número de Fit": [
	      "511"
	    ],
	    "Tipo de Producto": [
	      "Pantalones"
	    ]
	},
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
 | functionName     | String |   true   | "getOrder"                                    |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | orderId          | String |   true   | orderId of the order                          |
 | query       | String |   false  | the query that is needed to get the order (it can be an user id or some other thing depending on each integration)                     |

 #### Result Example
 ```json
{
	"order": [
		{
			"integration": "coolIntegration",
			"value": null
		},
		{
			"integration": "coolerIntegration",
			"value": {
				"statusCode": "picked-up",
				"statusFull": "Retirado",
				"estimatedDate": "2024-03-05T09:47:56-03:00"
			}
		}
	]
}
 ```
---

### getOrdersById
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "gerOrdersById"                               |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | query       | String |   false  | the query that is needed to get the order (it can be an user id or some other thing depending on each integration)                     |

 #### Result Example
 ```json
{
	"orderNumbers": [
		{
			"integration": "coolIntegration",
			"value": {}
		},
		{
			"integration": "coolerIntegration",
			"value": {
				"number1": "SLT-prs123-01",
				"number2": "pcs456-01",
				"number3": "pcs789-01",
				"number4": "pcs1011-01",
				"number5": "pcs1213-01",
			}
		}
	]
}
 ```
---
### getCart
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "getCart"                                     |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to return by conversation            |
 | cartId           | String |   false  | optional to return by cart id                 |

 #### Result Example
 ```json
{
	"cart": {
		"uuid": "e5fdca37-b40d-4445-986f-0d136c552579",
		"conversationUuid": "9d3598a5-62b0-465c-8855-90f76ef7b071",
		"products": [
			{
				"id": "19357",
				"title": "Set de Librería Mini Girls Mochila Negro",
				"description": "El Set de Librería Mini Girls Mochila Negro es la elección perfecta para las\nniñas que buscan estilo y organización en la escuela. Esta mochila de color\nnegro, con su diseño elegante y versátil, incluye una variedad de útiles\nescolares esenciales, desde cuadernos hasta lápices de colores, todo en un\npaquete sofisticado. La mochila ofrece comodidad y facilidad de transporte para\nllevar todos los suministros a clase, y su tono negro es perfecto para combinar\ncon cualquier atuendo. Con el Set de Librería Mini Girls Mochila Negro, tus\npequeñas estarán listas para afrontar el día escolar con estilo y funcionalidad.\nEste conjunto equilibra la moda con la practicidad, asegurando que tus niñas\nestén preparadas para sus actividades escolares mientras lucen geniales.",
				"link": "https://www.stylestore.com.ar/set-de-libreria-setminigirlsb/p",
				"quantity": 1,
				"cartProductId": "6",
				"products": [
					{
						"id": "21558",
						"price": 21895,
						"variants": [],
						"discount": 0,
						"images": [
							{
								"url": "https://stylewatch.vteximg.com.br/arquivos/ids/239127/Set_Liberia_Girls_Negra_01.jpg?v=638295261040700000"
							}
						],
						"isVisible": true,
						"isSelected": true
					}
				],
				"variants": []
			}
		],
		"createdAt": "2023-11-29T13:57:14.913Z",
		"updatedAt": null
	}
}
 ```
---

---

### createCart
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "createCart"                                  |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to return by conversation            |
 | products         | Array  |   true   | array of snappyCartProducts                   |

 #### Result Example
 ```json
{
	"cart": {
		"uuid": "e5fdca37-b40d-4445-986f-0d136c552579",
		"conversationUuid": "9d3598a5-62b0-465c-8855-90f76ef7b071",
		"products": [
			{
				"id": "19357",
				"title": "Set de Librería Mini Girls Mochila Negro",
				"description": "El Set de Librería Mini Girls Mochila Negro es la elección perfecta para las\nniñas que buscan estilo y organización en la escuela. Esta mochila de color\nnegro, con su diseño elegante y versátil, incluye una variedad de útiles\nescolares esenciales, desde cuadernos hasta lápices de colores, todo en un\npaquete sofisticado. La mochila ofrece comodidad y facilidad de transporte para\nllevar todos los suministros a clase, y su tono negro es perfecto para combinar\ncon cualquier atuendo. Con el Set de Librería Mini Girls Mochila Negro, tus\npequeñas estarán listas para afrontar el día escolar con estilo y funcionalidad.\nEste conjunto equilibra la moda con la practicidad, asegurando que tus niñas\nestén preparadas para sus actividades escolares mientras lucen geniales.",
				"link": "https://www.stylestore.com.ar/set-de-libreria-setminigirlsb/p",
				"quantity": 1,
				"cartProductId": "6",
				"products": [
					{
						"id": "21558",
						"price": 21895,
						"variants": [],
						"discount": 0,
						"images": [
							{
								"url": "https://stylewatch.vteximg.com.br/arquivos/ids/239127/Set_Liberia_Girls_Negra_01.jpg?v=638295261040700000"
							}
						],
						"isVisible": true,
						"isSelected": true
					}
				],
				"variants": []
			}
		],
		"createdAt": "2023-11-29T13:57:14.913Z",
		"updatedAt": null
	}
}
 ```
---

### addProductToCart
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "addProductToCart"                            |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to add to cart by conversation       |
 | cartId           | String |   false  | optional to add to cart by cart id            |
 | products         | Array  |   true   | array of snappyCartProducts                   |

 #### snappyCartProducts example
 ```json
{
  "productId": "123",
  "variantId": "abc",
  "quantity": "1"
}
 ```
 #### Result Example
 ```json
{
	"cart": {
		"uuid": "e5fdca37-b40d-4445-986f-0d136c552579",
		"conversationUuid": "9d3598a5-62b0-465c-8855-90f76ef7b071",
		"products": [
			{
				"id": "19357",
				"title": "Set de Librería Mini Girls Mochila Negro",
				"description": "El Set de Librería Mini Girls Mochila Negro es la elección perfecta para las\nniñas que buscan estilo y organización en la escuela. Esta mochila de color\nnegro, con su diseño elegante y versátil, incluye una variedad de útiles\nescolares esenciales, desde cuadernos hasta lápices de colores, todo en un\npaquete sofisticado. La mochila ofrece comodidad y facilidad de transporte para\nllevar todos los suministros a clase, y su tono negro es perfecto para combinar\ncon cualquier atuendo. Con el Set de Librería Mini Girls Mochila Negro, tus\npequeñas estarán listas para afrontar el día escolar con estilo y funcionalidad.\nEste conjunto equilibra la moda con la practicidad, asegurando que tus niñas\nestén preparadas para sus actividades escolares mientras lucen geniales.",
				"link": "https://www.stylestore.com.ar/set-de-libreria-setminigirlsb/p",
				"quantity": 1,
				"cartProductId": "6",
				"products": [
					{
						"id": "21558",
						"price": 21895,
						"variants": [],
						"discount": 0,
						"images": [
							{
								"url": "https://stylewatch.vteximg.com.br/arquivos/ids/239127/Set_Liberia_Girls_Negra_01.jpg?v=638295261040700000"
							}
						],
						"isVisible": true,
						"isSelected": true
					}
				],
				"variants": []
			}
		],
		"createdAt": "2023-11-29T13:57:14.913Z",
		"updatedAt": null
	}
}
 ```
---

### removeProductCart
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "addProductToCart"                            |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to remove from cart by conversation  |
 | cartId           | String |   false  | optional to remove from cart by cart id       |
 | cartProductId    | String |   true   | id of the cart product to remove              |

 #### Result Example
 ```json
{
	"cart": {
		"uuid": "e5fdca37-b40d-4445-986f-0d136c552579",
		"conversationUuid": "9d3598a5-62b0-465c-8855-90f76ef7b071",
		"products": [
			{
				"id": "19357",
				"title": "Set de Librería Mini Girls Mochila Negro",
				"description": "El Set de Librería Mini Girls Mochila Negro es la elección perfecta para las\nniñas que buscan estilo y organización en la escuela. Esta mochila de color\nnegro, con su diseño elegante y versátil, incluye una variedad de útiles\nescolares esenciales, desde cuadernos hasta lápices de colores, todo en un\npaquete sofisticado. La mochila ofrece comodidad y facilidad de transporte para\nllevar todos los suministros a clase, y su tono negro es perfecto para combinar\ncon cualquier atuendo. Con el Set de Librería Mini Girls Mochila Negro, tus\npequeñas estarán listas para afrontar el día escolar con estilo y funcionalidad.\nEste conjunto equilibra la moda con la practicidad, asegurando que tus niñas\nestén preparadas para sus actividades escolares mientras lucen geniales.",
				"link": "https://www.stylestore.com.ar/set-de-libreria-setminigirlsb/p",
				"quantity": 1,
				"cartProductId": "6",
				"products": [
					{
						"id": "21558",
						"price": 21895,
						"variants": [],
						"discount": 0,
						"images": [
							{
								"url": "https://stylewatch.vteximg.com.br/arquivos/ids/239127/Set_Liberia_Girls_Negra_01.jpg?v=638295261040700000"
							}
						],
						"isVisible": true,
						"isSelected": true
					}
				],
				"variants": []
			}
		],
		"createdAt": "2023-11-29T13:57:14.913Z",
		"updatedAt": null
	}
}
 ```
---
### createCheckout
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "createCheckout"                                 |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to create checkout by conversation   |
 | cartId           | String |   false  | optional to create checkout by cart id        |
 | shippingData     | object |   true   | snappyShippingData                            |


 #### snappyShippingData example
 ```json
{
  "firstName": "JuanoCruz", //required 
  "lastName": "Silva", //required
  "email": "test@testing.com" //required 
  "street": "Av.Corrientes",
  "number": "1234",
  "floor": "1",
  "locality": "CABA",
  "city": "CABA",
  "province": "CABA",
  "country": "AR",
  "postalCode": "1234",
  "phone": "1234567890",
  "pickupType": "pickup",
  "shipping": "No informado",
  "shippingOption": "No informado",
  "shippingCostCustomer": "0"
}
 ```
 #### Result Example
 ```json
{
	"checkoutData": "https://snappycommercestore.mitiendanube.com/checkout/v3/start/1391478941/7b8b44ef15b724cd85d2b01d9abdeccbcb56a436?from_store=1"
}
 ```
 ---
### createOrder
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "createOrder"                                 |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   true  | optional to create checkout by conversation   |
 | cartId           | String |   true  | optional to create checkout by cart id        |
 | shippingData     | object |   false   | snappyShippingData                            |


 #### shippingData example
 ```json
{
  "shippingProvince": "Buenos Aires",
  "shippingCity": "Pilar",
  "shippingPostalCode": 1629,
  "shippingStreet1": "Calle 123",
}
 ```
 #### Result Example
 ```json
{
	"order": {
		"id": 123138,
		"uuid": "8f076401-4655-42fc-ae9f-037d932d29a7",
		"number": 3791,
		"statusId": 1,
		"conversationId": 8730,
		"contactId": 140,
		"assignedTo": null,
		"createdBy": null,
		"shippingProvince": "Buenos Aires",
		"shippingCity": "Pilar",
		"shippingPostalCode": "1629",
		"shippingStreet1": "Calle 123",
		"shippingStreet2": null,
		"shippingCost": null,
		"subTotal": 47992,
		"createdAt": "2024-08-16T15:44:46.121Z",
		"updatedAt": "2024-08-16T15:44:46.095Z",
		"storeId": 1,
		"sectorId": 1,
		"total": 47992,
		"integrationOrderId": null,
		"solvedAt": null,
		"notes": null,
		"orderProducts": [
			{
				"id": 1,
				"orderId": 123138,
				"baseProductTitle": "Jeans Hombre",
				"productSku": "10",
				"productVariants": [
					3766,
					3758
				],
				"productSubVariants": [],
				"productImageUrl": "imageUrl",
				"price": 59990,
				"discount": 20,
				"amount": 1,
				"integrationProductId": "10"
			}
		]
	}
}
 ```

 ---
### fetchKnowledges
 #### Parameters
 | Parameter        | Type     | required | Description                                                                               |
 | ---------------- | -------- | ---------| ----------------------------------------------------------------------------------------- |
 | functionName     | String   |   true   | "fetchKnowledges"                                                                         |
 | botUuid          | String   |   true   | botUuid that is saved in the context                                                      |
 | query            | String   |   true   | words you wanna search by                                                                 |
 | amount           | Number   |   false  | amount of knowledges on the response                                                      |
 | maxDistance      | Number   |   false  | Max distance for the responses                                                            |
 | chunks           | Number   |   false  | Amount of chunks to bring around the matched one                                          |
 | categories       | String[] |   false  | Array of categories to filter by. Default Categories are `faqs`, `promotions`, `branches` |

 #### Result Example
 ```json
{
  "knowledges": [
    {
      "knowledge": "este es el texto 2",
      "distance": "0.0643550157547"
    },
    {
      "knowledge": "este es el texto 1",
      "distance": "0.0981845259666"
    },
    {
      "knowledge": "nuevo texto lucass",
      "distance": "0.146913230419"
    }
  ]
}
 ```
 ---

### fetchFaq
#### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "fetchFaq"                                    |
 | uuid             | String |   true   | The FAQ uuid                                  |

#### Result Example
```json
{
	"faq": {
		"uuid": "00df5ecf-0cac-46a1-80c7-cbfc2dcd50b3",
		"question": "¿A qué horario abren sus locales?",
		"answer": "Abrimos de lunes a viernes de 9 a 21hs!",
		"tags": [],
		"createdAt": "2024-04-22T21:17:15.586Z",
		"updatedAt": "2024-04-22T21:17:15.586Z"
	}
}
```

 ---

### fetchBranches
#### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "fetchBranches"                                    |
 | query            | String |   false  | Filter branches by the provided query (only will filter by name) |
 | address          | String |   false  | Order results by closest to the address |
 | point            | Object |   false  | Order results by closest to the point |
 | country          | String |   false  | Two letter ISO 3166-1 country code. Only applies when address is provided |

>[!NOTE]
>To fetch by point, parameter "point" would need the following format: { "lat": -40.123123, "lng": -50.12314 } with its values being of type "number".

#### Result Example
```json
{
	"branches": [
		{
			"uuid": "4979c755-e830-43b8-82d0-bbcb498ffee6",
			"name": "Algun Lugar 1",
			"address": "Champagnat 1050, Pilar, Buenos Aires, Argentina",
			"description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus id risus nec felis vehicula sodales. Pellentesque tortor est, egestas in tempor sed, pulvinar condimentum mi.",
			"phone": "+54 9 11 12344567",
			"schedules": [
				{
					"day": 0,
					"endTime": 1140,
					"startTime": 0
				},
				{
					"day": 1,
					"endTime": 1140,
					"startTime": 0
				},
				{
					"day": 2,
					"endTime": 1140,
					"startTime": 0
				},
				{
					"day": 3,
					"endTime": 900,
					"startTime": 750
				},
				{
					"day": 4,
					"endTime": 900,
					"startTime": 750
				},
				{
					"day": 3,
					"endTime": 660,
					"startTime": 540
				},
				{
					"day": 4,
					"endTime": 660,
					"startTime": 540
				}
			],
			"createdAt": "2024-06-14T15:16:58.866Z",
			"updatedAt": "2024-06-14T16:05:47.611Z",
			"latlng": {
				"lat": -34.445106,
				"lng": -58.9214688
			}
		}
	]
}
```
---

### fetchPromotions
#### Parameters
 | Parameter        | Type     | required | Description                                   |
 | ---------------- | -------- | ---------| --------------------------------------------- |
 | functionName     | String   |   true   | "fetchPromotions"                                    |
 | onlyActive       | Boolean  |   false  | Only fetch promotions that are currenlty active. Default is `true` |
 | validToday       | Boolean  | false    | Only fetch promotions that applies for today. Default is `false` |
 | days             | String[] | false    | Filter promotions by the provided days |
 | categories		| String[] | false	  | Filter promotions by the provided categories |
 
 >[!NOTE]
 >Valid days are `'sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday'`

 ```json
{
	"promotions": [
		{
			"uuid": "f802cfb8-0ddf-4b10-bb0f-b949acc3e3eb",
			"title": "Santander 20%",
			"description": "Todos los días tenes descuento con santander",
			"url": "https://www.santander.com.ar",
			"image": "https://www.santander.com.ar/banco/wcm/connect/9528486c-d93c-4b66-89b8-64e45d569e00/SLIDER_MODO_CINEPOLIS_Mobile.jpg?MOD=AJPERES&CACHEID=ROOTWORKSPACE-9528486c-d93c-4b66-89b8-64e45d569e00-ouAWtgu",
			"sinceDate": "2024-04-20T03:00:00.000Z",
			"untilDate": "2024-04-25T03:00:00.000Z",
			"days": [
				"monday",
				"tuesday",
				"wednesday",
				"thursday",
				"friday"
			],
			"createdAt": "2024-04-24T19:32:03.199Z",
			"updatedAt": "2024-04-25T14:13:46.401Z",
			"categories": ["diarco-barrio"],
			"branches": [],
			"imageAlt": "¡SÚPER DESCUENTO! 15% de descuento en cortes de carne congelada. Oferta válida del 13 al 19 de enero de 2025 en todas las sucursales de Diarco Barrio."
		}
	]
}
 ```

### fetchCategorySpecifications
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "fetchCategorySpecifications"                 |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | subCategoryId    | String |   true   | Search specifications of sub category         |

 #### Result Example
 ```json
{
  "specifications":
     [
	  {
	    "id": "116",
	    "name": "Cintura",
	    "values": [
	      "36",
	      "30",
	      "29"
	    ]
	  },
	  {
	    "id": "122",
	    "name": "Largo",
	    "values": [
	      "32",
	      "30",
	      "34"
	    ]
	  },
	  {
	    "id": "103",
	    "name": "Tipo de Producto",
	    "values": [
	      "Jeans",
	      "Buzos"
	    ]
	  }
    ]
}
 ```

 ---

 ## fetchCategories

#### Parameters

| Parameter    | Type   | required | Description                                         |
| ------------ | ------ | -------- | --------------------------------------------------- |
| functionName | String | true     | "fetchCategories"                                   |
| botUuid      | String | true     | botUuid that is saved in the context                |
| query        | String | false    | Checks for categories that include the given string |
| parentCategory | String | false | A parent category to get its children categories	 |
#### Result Example

```json
{
  "categories": [
	{
	  "id": "11",
	  "uuid": "c04bf496-661b-429b-98c9-a291442fba80",
	  "name": "toys",
	  "description": "Toys for people who enjoy playing with toys",
	  "value": "toys",
	  "label": "toys",
	  "canBeShared": true,
	  "url": "magictoys.com",
	  "subCategories": [
		{
		  "id": "9",
		  "uuid": "bb37e39d-ff33-4232-b5c4-b0bc6bebb734",
		  "categoryId": "11",
		  "name": "boys",
		  "createdAt": "2024-04-26T15:31:16.054Z",
		  "updatedAt": null,
		  "tsvName": "'boys':1A",
		  "productsCount": 0,
		  "value": "boys",
		  "label": "boys"
		},
		{
		  "id": "10",
		  "uuid": "0aff210a-9ab5-46af-b214-712be9b861ac",
		  "categoryId": "11",
		  "name": "girls",
		  "createdAt": "2024-04-26T15:31:22.480Z",
		  "updatedAt": null,
		  "tsvName": "'girls':1A",
		  "productsCount": 0,
		  "value": "girls",
		  "label": "girls"
		},
		{
		  "id": "12",
		  "uuid": "59f58dfc-55bb-4fc3-9197-db24d3c845df",
		  "categoryId": "11",
		  "name": "genderless",
		  "createdAt": "2024-04-26T15:31:35.170Z",
		  "updatedAt": null,
		  "tsvName": "'genderless':1A",
		  "productsCount": 0,
		  "value": "genderless",
		  "label": "genderless"
		},
		{
		  "id": "13",
		  "uuid": "538e7a85-55d4-4085-8da2-e792457cc47a",
		  "categoryId": "11",
		  "name": "adults",
		  "createdAt": "2024-04-26T15:31:44.781Z",
		  "updatedAt": null,
		  "tsvName": "'adults':1A",
		  "productsCount": 0,
		  "value": "adults",
		  "label": "adults"
		}
	  ],
	  "subCategoriesNames": ["boys", "girls", "genderless", "adults"]
	},
	{
	  "id": "12",
	  "uuid": "884c9c18-e8ce-4a1f-a394-1d730199af1f",
	  "name": "food",
	  "description": "",
	  "value": "food",
	  "label": "food",
	  "canBeShared": true,
	  "url": null,
	  "subCategories": [],
	  "subCategoriesNames": []
	}
  ],
  "categoriesNames": ["toys", "food"]
}
```

## executeIntegrationsProvider

#### Parameters

| Parameter    | Type   | required | Description                                         |
| ------------ | ------ | -------- | --------------------------------------------------- |
| functionName | String | true     | "executeIntegrationsProvider"                             |
| botUuid      | String | true     | botUuid that is saved in the context                |
| providerName | String | true     | Name of providers that wants to be executed         |
| parameters   | Object | false    | parameters that are needed to execute the provider  |

The only providerName allowed is "accountInformation" at the moment

#### Result Example

```json
[
	{
		"integration": "cresi",
		"value": {
			"name": "TARJETAS P BRENDA",
			"total": "224727",
			"minPayment": "0",
			"paymentLimit": "2024-06-03",
			"availablePoints": "1720",
			"recentMovements": [
				{
					"code": "CARGO       ",
					"date": "2024-05-27",
					"amount": "12499.00"
				},
				{
					"code": "CARGO       ",
					"date": "2024-05-24",
					"amount": "19343.00"
				},
				{
					"code": "CARGO       ",
					"date": "2024-05-23",
					"amount": "19343.00"
				},
				{
					"code": "CARGO       ",
					"date": "2024-05-20",
					"amount": "173542.00"
				},
				{
					"code": "CARGO       ",
					"date": "2024-05-15",
					"amount": "63992.00"
				}
			],
			"errorMessage": ""
		}
	}
]
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

