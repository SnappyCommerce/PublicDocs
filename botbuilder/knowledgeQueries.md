## Knowledge Queries (FAQs)
When the user asks the bot a question, the bot will use the user's input to search for knowledge related to that question to give the user a useful response. In order to help get a better result when searching for knowledge, the bot will now use standardized generic questions chosen based on the user's intent in addition to the user's input. This helps when the user makes a specific question that does not share common terms with the knowledge. For example: a user might ask 'do you take Visa?', for us, it is understood that the user is asking about payment methods, but if the client's knowledge does not mention Visa specifically, it might be hard for embeddings to find a relation between the user's input and the client's knowledge since there aren't any key terms shared between the input and the knowledge.  

To alleviate this, the bot will identify the user's intent and make an additional search for knowledge using a generic question based around common Frequently Asked Questions, such as 'What are your payment methods?', which will help find a better suited piece of knowledge to form a response that is useful to the user.
>[!IMPORTANT]
>It is recommended that clients include these queries as FAQs in their bot configuration.
---
These are the queries used for each intent:
| Language |
| --- |
| [1. Spanish](#spanish) |

### Spanish
| Intents | Query |
| --- | --- |
| addToCart, howToAddToCart, addToCartProblem | ¿Cómo agrego productos a mi carrito? |
| cantLogin | No puedo ingresar a mi cuenta |
| payInDues, installments | ¿Se puede pagar en cuotas? |
| wholesale | ¿Hacen ventas mayoristas? |
| howToGetInvoice, getInvoice | ¿Dónde puedo ver mi factura? |
| newsletter | ¿Qué es el newsletter? |
| signUpUser | ¿Cómo me registro? |
| webProblem | Tengo un problema con el sitio web |
| cancelOrder, howToCancelOrder | ¿Cómo cancelo un pedido? |
| createOrder, howToCreateOrder | ¿Cómo realizo una compra? |
| newArrivals | Quiero ver los últimos lanzamientos |
| pickUpOrder, pickUpInformation | ¿Puedo retirar mi pedido por una tienda? |
| eventEndDate | ¿Cuándo termina el evento? |
| getBranchesList, getNearestBranch | ¿Dónde puedo encontrar una tienda? |
| invoiceTypes | ¿Puedo pedir factura con mi pedido? |
| productSizes, sizesAvailable | Quiero ver la tabla de talles |
| shippingCost | ¿Cuánto cuesta el envío? |
| completeOrder | ¿Cómo finalizo la compra? |
| couponProblem | Tengo un problema con mi cupón de descuento |
| expiredCoupon | ¿Mi cupón de descuento está vencido? |
| myAccountInfo | ¿Dónde puedo ver los datos de mi cuenta? |
| requestReturn | ¿Cómo hago una devolución? |
| shippingTimes | ¿Cuáles son los plazos de envío? |
| bankPromotions, getPaymentPromotions | ¿Tienen alguna promoción con mi banco? |
| howToUseCoupon | ¿Cómo aplico mi cupón de descuento? |
| invoiceProblem | Tengo un problema con mi factura |
| taxInformation | ¿Los precios incluyen impuestos? |
| absentRecipient | ¿Qué pasa si no hay nadie para recibir el pedido? |
| incompleteOrder | ¿Qué hago si mi pedido llegó incompleto? |
| promotionIssues | Tengo un problema con las promociones |
| requestExchange, howToMakeExchange | ¿Cómo hago un cambio? |
| shippingMethods | ¿Cómo se hacen los envíos? |
| shippingProblem | Tengo un problema con el envío |
| addToCartProblem | No puedo agregar el producto que quiero al carrito |
| exchangePolicies | ¿Cuáles son las políticas de cambios y devoluciones? |
| howToTrackOrders, getOrderStatus | ¿Cómo hago el seguimiento de mi pedido? |
| orderFormProblem | Tengo un problema para finalizar la compra |
| viewOrderHistory, howToViewOrderHistory | ¿Cómo veo mi historial de compras? |
| deliverySchedules | ¿En qué horario se hace la entrega? |
| modifyAccountInfo, wrongAccountInfo | ¿Cómo cambio los datos de mi cuenta? |
| orderConfirmation | ¿Cómo sé cuando está confirmada mi orden? |
| orderDelayProblem | Mi pedido está atrasado |
| orderPriceProblem, productPriceProblem | Cuando voy a pagar aparece un precio distinto al esperado |
| resetUserPassword | ¿Cómo cambio mi contraseña? |
| getEventPromotions | ¿Qué promociones hay en el evento? |
| howToValidateEmail | ¿Cómo valido mi cuenta? |
| salesAndPromotions, getProductPromotions, getPromotionInformation | ¿Dónde puedo ver las promociones vigentes? |
| termsAndConditions | ¿Dónde puedo encontrar los términos y condiciones? |
| whereToPickUpOrder | ¿Por dónde puedo retirar mi pedido? |
| branchProductsStock | ¿Tienen el mismo stock en las sucursales que en la tienda online? |
| orderBillingProblem | Hubo un problema con el cobro |
| orderPaymentMethods | ¿Cuáles son los medios de pago? |
| orderPaymentProblem | Tengo un problema para hacer el pago |
| productStockProblem | El producto que quiero figura sin stock |
| subscribeNewsletter, howToSubscribeNewsletter | ¿Cómo me suscribo al newsletter? |
| getAttentionSchedule | ¿Cuáles son los horarios de atención al cliente? |
| submitJobApplication | Quiero trabajar con ustedes |
| websiteProductsStock | ¿Todos los productos de la tienda online están en stock? |
| firstPurchaseDiscount | ¿Hay descuento en la primera compra? |
| minimumPurchaseAmount | ¿Hay mínimo de compra? |
| modifyShippingAddress, shippingAddressProblem, howToModifyShippingAddress | ¿Cómo cambio la dirección de envío? |
| unsubscribeNewsletter | ¿Cómo me doy de baja del newsletter? |
| getOrderExchangeStatus | ¿Cómo puedo saber el estado de mi cambio? |
| getOrderTrackingNumber, orderTrackingNumberProblem, howToGetOrderTrackingNumber | ¿Dónde encuentro mi número de seguimiento? |
| receivedDamagedProduct | Recibí un producto en mal estado |
| whoCanReceiveMyPackage | ¿Puede recibir el pedido otra persona en mi lugar? |
| modifyOrderPaymentMethod | ¿Puedo cambiar el medio de pago de mi compra? |
| addProductToExistingOrder | ¿Puedo agregar un producto a mi orden? |
| orderDeliverySelectionProblem | No puedo elegir la forma de envío que quiero |
| taxInformation | ¿Los Impuestos están incluidos en el precio? |
| genericProductProblem | Tengo un problema con un producto |
