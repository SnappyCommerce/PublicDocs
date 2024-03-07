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

### addCartProducts
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

### removeCartProduct
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
 | functionName     | String |   true   | "createOrder"                                 |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | conversationUuid | String |   false  | optional to create checkout by conversation   |
 | cartId           | String |   false  | optional to create checkout by cart id        |
 | shippingData     | object |   true   | snappyShippingData                            |


 #### snappyShippingData example
 ```json
{
  "fristName": "JuanoCruz",
  "lastName": "Silva",
  "address": "Av.Corrientes",
  "number": "1234",
  "floor": "1",
  "locality": "CABA",
  "city": "CABA",
  "province": "CABA",
  "country": "AR",
  "zipcode": "1234",
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
### fetchKnowledges
 #### Parameters
 | Parameter        | Type   | required | Description                                   |
 | ---------------- | ------ | ---------| --------------------------------------------- |
 | functionName     | String |   true   | "fetchKnowledges"                             |
 | botUuid          | String |   true   | botUuid that is saved in the context          |
 | query            | String |   true   | words you wanna search by                     |
 | amount           | String |   false  | amount of knowledges on the response          |
 | maxDistance      | String |   false  | Max distance for the responses                |

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

