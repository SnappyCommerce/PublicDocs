# About
This document will explain what message types can be used and how they work.


# Message Types
Currently, there are two ways of sending a message from watson: **Generic** & **Snappy**

## Generic Output
Generic messages can be added from Watson's UI and are automatically inserted into the output JSON, therefore these cant be modified via the JSON to add aditional values.

> **Ordering Notice:** Generic outputs ordering is very limited, if very specific ordering is needed use Snappy's output

### Text Message
This message will output a simple text.

```json
"output": {
	"generic": [
		"values": [
				{
					"text": "$testKey"
				}
			],
		"response_type": "text",
		"selection_policy": "sequential"
	]
} 
```

### Options Message
The options message will display a question with buttons as answers. The buttons, when pressed, will automatically send the configured value as a text message to the bot.

> *For more customization use the options in Snappy's Output.*

```json
"output": {
	"generic": [
		{
			"title": "Option Title",
			"options": [
				{
					"label": "Option 1",
					"value": {
						"input": {
							"text": "1"
						}
					}
				},
				{
					"label": "Option 2",
					"value": {
						"input": {
							"text": "2"
						}
					}
				},
				{
					"label": "Option 3",
					"value": {
						"input": {
							"text": "3"
						}
					}
				}
			],
			"description": "Option Description",
			"response_type": "option"
		}
	]
}
```
### User Defined
This message allows to send snappy outputs through the generic output (workaround for wierd watson bug)
Multiple outputs:
```json
{
  "output": {
    "generic": [
      {
        "user_defined": {
          "type": "array",
          "items": [
            {
              "text": "hola",
              "type": "text"
            },
            {
              "text": "chau",
              "type": "text"
            }
          ]
        },
        "response_type": "user_defined"
      }
    ]
  }
}
```

Single output:
```json
{
  "output": {
    "generic": [
      {
        "user_defined": {
          "text": "hola",
          "type": "text"
        },
        "response_type": "user_defined"
      }
    ]
  }
}
```
## Snappy Output
Due to a limitation in Watson, a second output group that could handle any type of message was needed and **Snappy's Output** was born.

Using this output, you can send any message and order them in any way you want.

### Text Message
This message will output a simple text.

```json
"output": {
	"snappy": [
		"text": "Example Output Text"
		"type": "text",
		"priority": 1
	]
}
```

### Options Message
The options message will display a question with buttons as answers. The buttons, when pressed, will automatically send the configured value as a text message to the bot.

The horizontal value defines in what way should the options be displayed.
- **false** *(Default)*: Shows the options as a vertical list.
- **true**: Shows the options as horizontal separate buttons.

```json
"output": {
	"snappy": [
		{
			"title": "Option Title",
			"options": [
				{
					"label": "Option 1",
					"value": "1"
				},
				{
					"label": "Option 2",
					"value": "2"
				},
				{
					"label": "Option 3",
					"value": "3"
				},
			],
			"description": "Option Description",
			"type": "option",
			"priority": 2,
			"horizontal": true
		}
	]
}
```

### Product Search Message
The search message executes a product search when sent, the query to search is specified in the query.

```json
"output": {
	"snappy": [
		{
			"values": {
				query: "Remera" (opcional)
				categoryId: "1" (opcional)
				subCategoryId: "2" (opcional)
			},
			"type": "productSearch",
			"priority": 3
		}
	]
}
```

### Quick Reply Message
The quick reply message sends a customer set quick reply.

```json
"output": {
	"snappy": [
		{
			"value": 1,
			"type": "quickreply",
			"priority": 4
		}
	]
}
```

### Promotions Message
Sends the stores current promotions

```json
"output": {
	"snappy": [
		{
			"type": "promotions",
			"priority": 0
		}
	]
}
```

### Carousel Message
Sends a custom carousel of images with names and links.

```json
"output": {
	"snappy": [
		{
			"items": [
				{
					"name": "Google",
					"link": "https://google.com",
					"image": "https://example.com/image.jpg"
				},
				{
					"name": "Bing",
					"link": "https://bing.com",
					"image": "https://example.com/image.jpg"
				}
			],
			"type": "carousel",
			"priority": 5
		}
	]
}
```

### Carousel Products Message
Sends a custom carousel of predefined products.

```json
"output": {
	"snappy": [
		{
			"products": [
				{
				    "id": "1062",
				    "title": "Pantalón Hombre Levi's XX CHINO EZ Taper",
				    "description": "Nuestro pantalón Chino EZ Taper cómodo y suave, está aquí¡ para brindarte un\najuste regular que te favorecerá.",
				    "detailedDescription": "",
				    "link": "https://www.levi.cl/pantalon-hombre-levi-s-xx-chino-ez-taper-a1041-0010/p",
				    "products": [
					      {
						"id": "9395",
						"price": 54990,
						"variants": [
						  "3762",
						  "3758"
						],
						"discount": 40,
						"images": [
						  {
						    "url": "https://leviscl.vteximg.com.br/arquivos/ids/1021914/A1041-0010_1.jpg?v=637949624130230000"
						  }
						],
						"isVisible": true
					      },
					      {
						"id": "9396",
						"price": 54990,
						"variants": [
						  "3757",
						  "3758"
						],
						"discount": 40,
						"images": [
						  {
						    "url": "https://leviscl.vteximg.com.br/arquivos/ids/1021918/A1041-0010_1.jpg?v=637949624154400000"
						  }
						],
						"isVisible": true
					      },
					      {
						"id": "9397",
						"price": 54990,
						"variants": [
						  "3766",
						  "3758"
						],
						"discount": 40,
						"images": [
						  {
						    "url": "https://leviscl.vteximg.com.br/arquivos/ids/1021922/A1041-0010_1.jpg?v=637949624176900000"
						  }
						],
						"isVisible": true
					      },
					      {
						"id": "9398",
						"price": 54990,
						"variants": [
						  "3759",
						  "3758"
						],
						"discount": 40,
						"images": [
						  {
						    "url": "https://leviscl.vteximg.com.br/arquivos/ids/1021926/A1041-0010_1.jpg?v=637949624201000000"
						  }
						],
						"isVisible": true
					      }
					    ],
					    "variants": [
					      {
						"id": "3759",
						"type": "Cintura",
						"value": "30"
					      },
					      {
						"id": "3762",
						"type": "Cintura",
						"value": "32"
					      },
					      {
						"id": "3766",
						"type": "Cintura",
						"value": "34"
					      },
					      {
						"id": "3757",
						"type": "Cintura",
						"value": "36"
					      },
					      {
						"id": "3758",
						"type": "Largo",
						"value": "32"
					      }
					    ]
				  }
			],
			"type": "carousel",
			"priority": 5
		}
	]
}
```

### Store Search Message
Sends a map with nearby stores and their information.

```json
"output": {
	"snappy": [
		{
			
			"type": "storeSearch",
			"value": "San Isidro, Buenos Aires, Argentina",
			"priority": 6
		}
	]
}
```

### Order Status Message
Sends a message containing the order status.

> **Async:** On supported platforms an initial message will be sent and its data will later change.

```json
"output": {
	"snappy": [
		{
			
			"type": "orderStatus",
			"value": "966382136547-01",
			"priority": 7
		}
	]
}
```

### FAQ Message
Sends the store's FAQ.
> **Value:** The value of this message is the desired FAQ id you wish to send.
```json
"output": {
	"snappy": [
		{
			"type": "faq",
			"value": 1,
			"priority": 8
		}
	]
}
```

### Assist 365 Message
Sends Assist365's custom message.
```json
"output": {
	"snappy": [
		{
			"type": "assist365",
			"priority": 9
		}
	]
}
```

## Events
Events are outputs that execute actions without necessarily sending a message.

### Assistance Request Action
Notifies the system that the user is requesting assistance.

```json
"output": {
	"snappy": [
		{
			"type": "requestAssistance"
		}
	]
}
```

### Google Analytics Event
This event sends an Analytic Event.

> Compatibility: Only works on web
> 
> Output: This event does not respond anything.

```json
"output": {
	"snappy": [
		{
			"type": "analyticsEvent"
		}
	]
}
```

