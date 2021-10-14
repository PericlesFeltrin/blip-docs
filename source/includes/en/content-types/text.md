## Plain Text
| MIME type  |
|------------|
| text/plain |

Allows sending and receiving simple text messages.

> Sending a message to a Messenger recipient:

```javascript
client.sendMessage({
    id: Lime.Guid(),
    type: "text/plain",
    to: "128271320123982@messenger.gw.msging.net",
    content: "Welcome to our service! How can I help you?"
    });
```

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Messaging.Contents;
using Lime.Protocol;
using Take.Blip.Client;

//Replying a received message with a simple text message.
public class PlainTextMessageReceiver : IMessageReceiver
{
private readonly ISender _sender;
private readonly Settings _settings;

public PlainTextMessageReceiver(ISender sender, Settings settings)
{
    _sender = sender;
    _settings = settings;
}

public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
{
    var document = new PlainText {Text = "Welcome to our service! How can I help you?"};
    await _sender.SendMessageAsync(document, message.From, cancellationToken);
}
}
```

```python
client.send_message(
    Message.from_json(
        {
            'id': '1',
            'to': '128271320123982@messenger.gw.msging.net',
            'type': 'text/plain',
            'content': 'Welcome to our service! How can I help you?'
        }
    )
)
```

```http
POST https://http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
    "id": "1",
    "to": "128271320123982@messenger.gw.msging.net",
    "type": "text/plain",
    "content": "Welcome to our service! How can I help you?"
}
```

For more details, check the specification of [LIME protocol](http://limeprotocol.org/content-types.html#text).

<aside class="notice">
Note: Some channels may have a character limit per message.
</aside>

| Messenger                         | BLiPChat                           |
|-----------------------------------|------------------------------------|
| ![imagem](images/text_mssngr.png) | ![imagem](images/textBlipChat.png) |

#### Channel mapping

| Channel   | Type                                                                                                    |
|-----------|---------------------------------------------------------------------------------------------------------|
| Blip Chat | Text                                                                                                    |
| Messenger | [Text message](https://developers.facebook.com/docs/messenger-platform/send-api-reference/text-message) |
| Whatsapp  | Text                                                                                                    |
| SMS       | Text                                                                                                    |
| Skype     | [Activity](https://docs.botframework.com/en-us/skype/chat/#sending-messages-1)                          |
| Telegram  | [Message](https://core.telegram.org/bots/api#message)                                                   |

