## Media link

> Sending the link of an image including title, descriptive text and metadata:

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Messaging.Contents;
using Lime.Protocol;
using Take.Blip.Client;

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
    var imageUri = new Uri("http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg", UriKind.Absolute);
    var previewUri = new Uri("https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcS8qkelB28RstsNxLi7gbrwCLsBVmobPjb5IrwKJSuqSnGX4IzX", UriKind.Absolute);

    var document = new MediaLink
    {
        Title = "Cat",
        Text = "Here is a cat image for you!",
        Type = MediaType.Parse("image/jpeg"),
        AspectRatio = "1:1",
        Size = 227791,
        Uri = imageUri,
        PreviewUri = previewUri
    };

    await _sender.SendMessageAsync(document, message.From, cancellationToken);
}
}
```

```http
POST https://http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
    "id": "1",
    "to": "553199991111@0mn.io",
    "type": "application/vnd.lime.media-link+json",
    "content": {
        "title": "Cat",
        "text": "Here is a cat image for you!",
        "type": "image/jpeg",
        "uri": "http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg",
        "aspectRatio": "1:1",
        "size": 227791,
        "previewUri": "https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcS8qkelB28RstsNxLi7gbrwCLsBVmobPjb5IrwKJSuqSnGX4IzX",
        "previewType": "image/jpeg"
    }
}
```

```javascript
    client.sendMessage({
      id: Lime.Guid(),
      type: "application/vnd.lime.media-link+json",
      to: "128271320123982@messenger.gw.msging.net",
      content: {
        title: "Cat",
        text: "Here is a cat image for you!",
        type: "image/jpeg",
        uri: "http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg",
        aspectRatio: "1:1",
        size: 227791,
        previewUri: "https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcS8qkelB28RstsNxLi7gbrwCLsBVmobPjb5IrwKJSuqSnGX4IzX",
        previewType: "image/jpeg"
      }
    });
```

```python
client.send_message(
    Message.from_json(
        {
            'id': '1',
            'type': 'application/vnd.lime.media-link+json',
            'to': '128271320123982@messenger.gw.msging.net',
            'content': {
                'title': 'Cat',
                'text': 'Here is a cat image for you!',
                'type': 'image/jpeg',
                'uri': 'http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg',
                'aspectRatio': '1:1',
                'size': 227791,
                'previewUri': 'https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcS8qkelB28RstsNxLi7gbrwCLsBVmobPjb5IrwKJSuqSnGX4IzX',
                'previewType': 'image/jpeg'
            }
        }
    )
)
```

> Sending an audio link: (For more details, check the [LIME protocol](http://limeprotocol.org/content-types.html#media-link) specification)

```csharp
var audioMediaLink = new MediaLink
{
    Title = "Audio",
    Type = MediaType.Parse("audio/mp3"),
    Uri = new Uri("http://blaamandagjazzband.dk/jazz/mp3/basin_street_blues.mp3"),
    Size = 3124123,
    AspectRatio = "1:1"
};

await _sender.SendMessageAsync(audioMediaLink, message.From, cancellationToken);
```

```python
client.send_message(
    Message.from_json(
        {
            'id': '2',
            'to': '553199991111@0mn.io',
            'type': 'application/vnd.lime.media-link+json',
            'content': {
                'type': 'audio/mp3',
                'uri': 'http://blaamandagjazzband.dk/jazz/mp3/basin_street_blues.mp3',
                'size': 3124123
            }
        }
    )
)
```

```http
POST https://http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
    "id": "2",
    "to": "553199991111@0mn.io",
    "type": "application/vnd.lime.media-link+json",
    "content": {
        "type": "audio/mp3",
        "uri": "http://blaamandagjazzband.dk/jazz/mp3/basin_street_blues.mp3",
        "size": 3124123
    }
}
```

| MIME type                            |
|--------------------------------------|
| application/vnd.lime.media-link+json |

Allows sending and receiving of links for multimedia content. The link can be any valid **URI**, but most part of the channels only support content served by **HTTP/HTTPS** protocol. It is possible to include a title and a text, besides image *metadada* such as MIME type, size and *preview*.

Some channels allow defining the display's *aspect ratio* for certain media types. For instance, in *Messenger*, you should set the `1:1` value for the `aspectRatio` property to send squared images.

<aside class="notice">
Note: metadata support varies per channel. It may be ignored if not supported.
</aside>

```javascript
    client.sendMessage({
      id: Lime.Guid(),
      type: "application/vnd.lime.media-link+json",
      to: "128271320123982@messenger.gw.msging.net",
      content: {
        type: "audio/mp3",
        uri: "http://blaamandagjazzband.dk/jazz/mp3/basin_street_blues.mp3",
        size: 3124123
      }
    });
```

For more details, check the [LIME protocol](http://limeprotocol.org/content-types.html#media-link) specification.

#### Channel mapping

| Channel   | Type                                                                                                                                                        |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Blip Chat | Media Link                                                                                                                                                  |
| Messenger | [Attachments](https://developers.facebook.com/docs/messenger-platform/send-api-reference/image-attachment) (image/audio/video/file, depending of MIME type) |
| Whatsapp  | Media Link                                                                                                                                                  |
| SMS       | Text with link                                                                                                                                              |
| Skype     | [Activity](https://docs.botframework.com/en-us/skype/chat/#sending-messages-1)                                                                              |
| Telegram  | [Message](https://core.telegram.org/bots/api#message)                                                                                                       |

