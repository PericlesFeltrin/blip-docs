### Images

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

    Document document = new MediaLink
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

```http
POST https://{{contract.id}}.http.msging.net/messages HTTP/1.1
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
You can send images by uploading them or sharing a URL using the [Media Link](/#media-link) content type. Supported formats are jpg, png and gif.

| Messenger                 | BLiPChat                     |
|---------------------------|------------------------------|
| ![imagem](img_mssngr.png) | ![imagem](imageBlipChat.png) |
