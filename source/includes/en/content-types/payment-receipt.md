## Payment receipt

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Messaging.Contents;
using Lime.Protocol;
using Take.Blip.Client;

public class InvoiceStatusReceiver : IMessageReceiver
{
private readonly IMessagingHubSender _sender;

public InvoiceStatusReceiver(IMessagingHubSender sender)
{
    _sender = sender;
    _settings = settings;
}

public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
{
    var invoiceStatus = message.Content as InvoiceStatus;
    switch (invoiceStatus?.Status)
    {
        case InvoiceStatusStatus.Cancelled:
            await _sender.SendMessageAsync("Ok, you don't need pay anything.", message.From, cancellationToken);
            break;
        case InvoiceStatusStatus.Completed:
            await _sender.SendMessageAsync("Thank you for your payment, this is only a test", message.From, cancellationToken);
            var document = new PaymentReceipt
            {
                Currency = "BLR",
                Total = 10
                Items =
                    new[]
                    {
                        new InvoiceItem
                        {
                            Currency = "BRL",
                            Unit = 10,
                            Description = "Item 1",
                            Quantity = 10,
                            Total = 10
                        }
                    },
            };
            await _sender.SendMessageAsync(document, message.From, cancellationToken);
            break;
        case InvoiceStatusStatus.Refunded:
            await _sender.SendMessageAsync("Ok, your payment was refunded by PagSeguro!", message.From, cancellationToken);
            break;
    }
}
}
```

```javascript
    client.sendMessage({
      id: Lime.Guid(),
      type: "application/vnd.lime.payment-receipt+json",
      to: "128271320123982@messenger.gw.msging.net",
      content: {
        paidOn: "2016-08-26T19:03:37.024Z",
        code: "215BF6B5-01EF-4F9A-A944-0BC05FD0F228",
        method: {
                name: "Credit Card"
        },
        currency: "BRL",
        total: 10.85,
        items: [{
                quantity: 1.0,
                unit: 10.85,
                currency: "BRL",
                total: 10.85,
                description: "Item 1"
            }
        ]
    }
    });
```

```python
client.send_message(
    Message.from_json(
        {
            'id': '1',
            'to': '1042221589186385@messenger.gw.msging.net',
            'type': 'application/vnd.lime.payment-receipt+json',
            'content': {
                'paidOn': '2016-08-26T19:03:37.024Z',
                'code': '215BF6B5-01EF-4F9A-A944-0BC05FD0F228',
                'method': {
                        'name': 'Credit Card'
                },
                'currency': 'BRL',
                'total': 10.85,
                'items': [{
                        'quantity': 1.0,
                        'unit': 10.85,
                        'currency': 'BRL',
                        'total': 10.85,
                        'description': 'Item 1'
                    }
                ]
            }
        }
    )
)
```

```http
POST https://{{contract.id}}.http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
    "id": "1",
    "to": "1042221589186385@messenger.gw.msging.net",
    "type": "application/vnd.lime.payment-receipt+json",
    "content": {
        "paidOn": "2016-08-26T19:03:37.024Z",
        "code": "215BF6B5-01EF-4F9A-A944-0BC05FD0F228",
        "method": {
                "name": "Credit Card"
        },
        "currency": "BRL",
        "total": 10.85,
        "items": [{
                "quantity": 1.0,
                "unit": 10.85,
                "currency": "BRL",
                "total": 10.85,
                "description": "Item 1"
            }
        ]
    }
}
```

| MIME type                                 |
|-------------------------------------------|
| application/vnd.lime.payment-receipt+json |

Allows sending of a payment receipt to a customer.

In order to realize a payment on your chatbot, it is necessary to use the payment channel. For now, only the PagSeguro channel is supported. To request a payment, the chatbot must send a message of type Invoice to the payment channel informing the user address using the format at right bar.

#### Example
Sending a payment receipt to a Messenger user:



#### Channel mapping

| Channel   | Type                                                                                                            |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| Blip Chat | Not supported yet                                                                                               |
| Messenger | [Receipt template](https://developers.facebook.com/docs/messenger-platform/send-api-reference/receipt-template) |
| Whatsapp  | Text                                                                                                            |
| SMS       | Text                                                                                                            |
| Skype     | [Activity](https://docs.botframework.com/en-us/skype/chat/#sending-messages-1)                                  |
| Telegram  | [Message](https://core.telegram.org/bots/api#message)                                                           |
