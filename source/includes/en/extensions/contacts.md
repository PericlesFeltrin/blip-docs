## Contacts

The **contacts** extension allows the management of the chatbot's roster, which can be used to store data of the chatbot's clients. It is possible to save information like name, address, gender and other generic information, using the `extras` property. It is also possible to use the contacts fields as variables of the messages sent by the chatbot. This property only allows `string` values and does not allows complex objects. You can also set the `group` property for contacts organization. Events where the `identity` property is from a special group called 'testers' will be ignored on Blip events dashboard.

To use any feature of **contacts** extension send a command with the following properties:

| Name     | Description                              |
|----------|------------------------------------------|
| id       | Unique identifier of the command.        |
| method   | The command verb                         |
| resource | The contact document.                    |
| type     | **"application/vnd.lime.contact+json"**  |
| uri      | **/contacts**                            |
| to       | **postmaster@msging.net** (not required) |

The command's properties `resource` and `method` can change according to the feature.
A [contact object](/#contact) passed as a document `resource` has the following properties:

| Property     | Description                                                        | Example |
|--------------|--------------------------------------------------------------------|---------|
| **identity** | The client identity in a specific channel.                         | `11121023102013021@messenger.gw.msging.net (Messenger user)` |
| **name**   | **Optional** The client's name (string).  | `"Rafael Pacheco"` |
| **gender** | **Optional** The client's gender (string).  | `"male"` |
| **group**   | **Optional** The client's group tag (string).   | `"testers"` |
| **address**   | **Optional** The client's address (string).   | `"83, Paraguassu Street"` |
| **city**   | **Optional** The client's city (string).   | `"Belo Horizonte"` |
| **email**   | **Optional** The client's email (string).   | `"rafaelpa@take.net"` |
| **phoneNumber**   | **Optional** The client's phone number (string).   | `"5531000000000"` |
| **cellPhoneNumber**   | **Optional** The client's cell phone number (string).   | `"5531999999999"` |
| **timezone**   | **Optional** The client's timezone id (int).   | `-3` |
| **culture**   | **Optional** The client's culture info (string).   | `"pt-br"` |
| **extras**   | **Optional** The client's extra informations.         | `{"customerExternalId": "41231", "cpf": "00000000000" }` |
| **source**   | **Optional** The client's source (channel) info (string).   | `"Facebook Messenger"` |
| **lastMessageDate**   | **Optional** The client's last interaction (datetimeoffset).   | `2021-09-30T13:38:00.000Z` |
| **taxDocument** | **Optional** the client's identification document code (string). | `"00000000000"`|

For more information about the supported fields, please refer to the [Lime protocol](http://limeprotocol.org/resources.html#contact) documentation.

#### Message variable replacement

The contacts fields can be used to replace variables on messages sent by the chatbot. To make a replacement in a message, the `metadata` key `#message.replaceVariables` should be present with the value `true` and the message text should have variables in the `${contact.<propertyName>}` format, where `<propertyName>` is the contact property for replacement. It is possible to use all fields from the contact, including the keys in the `extras` property. In this case, is only required to use the `${contact.extras.<extraPropertyName>}` convention, where `<extraPropertyName>` is the value for replacement. If the value is not available, it is only removed from the message.

### Add (or update) a contact

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    await client.sendCommand({  
        id: Lime.Guid(),
        method: Lime.CommandMethod.SET,
        uri: '/contacts',
        type: 'application/vnd.lime.contact+json',
        resource: {
            identity: '{{$user_identity}}',
            name: '{{$user_name}}',
            gender:'{{$user_gender}}',
            group: '{{$user_groups}}',
            extras: {
                plan: 'Gold',
                code: '1111'
            },
            "source": "{{$user_channel_name}}"
        }
    });
});
```

```python
async def message_receiver_async(message: Message) -> None:
    result = await client.process_command_async(
        Command(
            CommandMethod.SET,
            '/contacts',
            'application/vnd.lime.contact+json',
            {
                'identity': '{{$user_identity}}',
                'name': '{{$user_name}}',
                'gender':'{{$user_gender}}',
                'group': '{{$user_groups}}',
                'extras': {
                    'plan': 'Gold',
                    'code': '1111'
                },
                "source": "{{$user_channel_name}}"
            }
        )
    )

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver_async))
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "method": "set",
  "uri": "/contacts",
  "type": "application/vnd.lime.contact+json",
  "resource": {
    "identity": "{{$user_identity}}",
    "name": "{{$user_name}}",
    "gender":"{{$user_gender}}",
    "group":"{{$user_group}}",    
    "extras": {
      "plan":"Gold",
      "code":"1111"      
    },
    "source": "{{$user_channel_name}}"
  }
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "method": "set",
    "status": "success",
    "id": "1",
    "from": "postmaster@crm.msging.net/#az-iris5",
    "to": "contact@msging.net",
    "metadata": {
        "#command.uri": "lime://contact@msging.net/contacts"
    }
}
```

```csharp
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Take.Blip.Client.Extensions.Contacts;
using Lime.Messaging.Resources;
using System.Collections.Generic;

namespace Extensions
{
    public class ContactMessageReceiver : IMessageReceiver
    {
        private readonly ISender _sender;
        private readonly Settings _settings;
        private readonly IContactExtension _contactExtension;

        public ContactMessageReceiver(ISender sender, Settings settings, IContactExtension contactExtension)
        {
            _sender = sender;
            _settings = settings;
            _contactExtension = contactExtension;
        }

        public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
        {
            var identity = new Identity("11121023102013021", "messenger.gw.msging.net");
            var contact = new Contact
            {
                Name = "John Doe",
                Gender = Gender.Male,
                Group = "friends",
                Extras = new Dictionary<string, string>
                {
                    {"plan", "gold" },
                    {"code", "1111" },
                },
                Source = "Facebook Messenger"
            };

            await _contactExtension.SetAsync(identity, contact, cancellationToken);
        }
    }
}
```

In order to store informations about a chatbot's client, it is possible to save and update data using **contacts extension**.

It is necessary to send the requisition according to the [contact object](/#contact) with the following properties:

| Property            | Description                                                                         | Example                                                      |
|---------------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------|
| **identity**        | The client identity in a specific channel.                                          | `11121023102013021@messenger.gw.msging.net (Messenger user)` |
| **name**            | **Optional** The client's name (string).                                            | `"Rafael Pacheco"`                                           |
| **gender**          | **Optional** The client's gender (string).                                          | `"male"`                                                     |
| **group**           | **Optional** The client's group tag (string).                                       | `"testers"`                                                  |
| **address**         | **Optional** The client's address (string).                                         | `"83, Paraguassu Street"`                                    |
| **city**            | **Optional** The client's city (string).                                            | `"Belo Horizonte"`                                           |
| **email**           | **Optional** The client's email (string).                                           | `"rafaelpa@take.net"`                                        |
| **phoneNumber**     | **Optional** The client's phone number (string).                                    | `"5531000000000"`                                            |
| **cellPhoneNumber** | **Optional** The client's cell phone number (string).                               | `"5531999999999"`                                            |
| **timezone**        | **Optional** The client's timezone id (int).                                        | `-3`                                                         |
| **culture**         | **Optional** The client's culture info (string).                                    | `"pt-br"`                                                    |
| **extras**          | **Optional** The client's extra informations.                                       | `{"customerExternalId": "41231", "cpf": "00000000000" }`     |
| **source**          | **Optional** The client's source (channel) info (string). Check [here](/#channels). | `"Facebook Messenger"`                                       |

<aside  class="notice">
When updating a contact, not passing one of the existing parameters for the Contact in the request will delete it.<br><br><i>For example, if the Contact already has an Email parameter and you don't pass It in the requisition, the contact email information will be deleted.</i>
</aside>

### Add a comment

Add a new comment for a contact ( `contactIdentity` ), using a [Comment](/#comment) document.

```python
result = await client.process_command_async(
    Command(
        CommandMethod.SET,
        '/contacts/{contactIdentity}/comments',
        'application/vnd.iris.crm.comment+json',
        {
            'content': 'This is a comment example'
        },
        id='{{$guid}}'
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@crm.msging.net",
  "method": "set",
  "uri": "/contacts/{contactIdentity}/comments",
  "type": "application/vnd.iris.crm.comment+json",
  "resource": {
    "content": "This is a comment example"
  }
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.iris.crm.comment+json",
    "resource": {
        "id": "3d24afee-898c-47b8-9de6-8d4bfa8fb93a",
        "storageDate": "2019-11-27T19:36:04.076Z",
        "content": "This is a comment example"
    },
    "method": "set",
    "status": "success",
    "id": "e38ba651-cd35-4a1d-a9e7-4c081f5cf2bb",
    "from": "postmaster@crm.msging.net/#az-iris1",
    "to": "demobot@msging.net",
}
```

### Delete a comment

Delete a specific comment for a contact.

Replace `commentId` with the comment Id.

```python
result = await client.process_command_async(
    Command(
        CommandMethod.DELETE,
        '/contacts/{contactIdentity}/comments/{commentId}',
        id='{{$guid}}'
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@crm.msging.net",
  "method": "delete",
  "uri": "/contacts/{contactIdentity}/comments/{commentId}"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "method": "delete",
    "status": "success",
    "id": "fe76fdb3-d4be-4969-aad3-9303d4897776",
    "from": "postmaster@crm.msging.net/#az-iris3",
    "to": "demobot@msging.net",
}
```

### Get comments

Get all comments for a contact ( `contactIdentity` ). By default, Blip will return the last 100 comments.

| QueryString | Description                                                              |
|-------------|--------------------------------------------------------------------------|
| $skip       | The number of elements to be skipped                                     |
| $take       | Limit of total of items to be returned. The maximum value allowed is 100 |

```python
result = await client.process_command_async(
    Command(
        CommandMethod.GET,
        '/contacts/{contactIdentity}/comments',
        id='{{$guid}}'
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@crm.msging.net",
  "method": "get",
  "uri": "/contacts/{contactIdentity}/comments"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.collection+json",
    "resource": {
        "total": 2,
        "itemType": "application/vnd.iris.crm.comment+json",
        "items": [
            {
                "id": "03232ff4-2e0b-4d89-8440-7f888983048d",
                "storageDate": "2019-10-24T14:10:16.820Z",
                "content": "I love this guy!!"
            },
            {
                "id": "3d24afee-898c-47b8-9de6-8d4bfa8fb93a",
                "storageDate": "2019-11-27T19:36:04.080Z",
                "content": "This is a comment example"
            }
        ]
    },
    "method": "get",
    "status": "success",
    "id": "6f85587e-f58a-42c3-90b4-a2b603a7ed65",
    "from": "postmaster@crm.msging.net/#az-iris2",
    "to": "demobot@msging.net"
}
```

### Get contact

```python
async def message_receiver_async(message: Message) -> None:
    result = await client.process_command_async(
        Command(
            CommandMethod.GET,
            '/contacts/11121023102013021@messenger.gw.msging.net',
            id='{{$guid}}'
        )
    )

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver_async))
```

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    var data = await client.sendCommand({  
        id: Lime.Guid(),
        method: Lime.CommandMethod.GET,
        uri: '/contacts/11121023102013021@messenger.gw.msging.net'
    });
    console.log(data);
});
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "method": "get",
  "uri": "/contacts/11121023102013021@messenger.gw.msging.net"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.contact+json",
    "resource": {
        "name": "John Doe",
        "group": "friends",
        "identity": "11121023102013021@messenger.gw.msging.net",
        "gender": "male",
        "extras": {
            "plan": "Gold",
            "code": "1111"
        }
    },
    "method": "get",
    "status": "success",
    "id": "2",
    "from": "postmaster@crm.msging.net/#az-iris3",
    "to": "contact@msging.net/default",
    "metadata": {
        "#command.uri": "lime://contact@msging.net/contacts/11121023102013021@messenger.gw.msging.net"
    }
}
```

```csharp
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Take.Blip.Client.Extensions.Contacts;

namespace Extensions
{
    public class ContactMessageReceiver : IMessageReceiver
    {
        private readonly ISender _sender;
        private readonly IContactExtension _contactExtension;
        private readonly Settings _settings;

        public ContactMessageReceiver(ISender sender, Settings settings, IContactExtension contactExtension)
        {
            _sender = sender;
            _settings = settings;
            _contactExtension = contactExtension;
        }

        public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
        {
            var identity = new Identity("11121023102013021", "messenger.gw.msging.net");

            var contact = await _contactExtension.GetAsync(identity, cancellationToken);
        }
    }
}
```

For the same contact `11121023102013021@messenger.gw.msging.net`, it is possible to get all of its information using a `GET` contact command.

### Get contacts

```python
async def message_receiver_async(message: Message) -> None:
    result = await client.process_command_async(
        Command(
            CommandMethod.GET,
            '/contacts?$skip=0&$take=3',
            id='{{$guid}}'
        )
    )

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver_async))
```

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    var data = await client.sendCommand({  
        id: Lime.Guid(),
        method: Lime.CommandMethod.GET,
        uri: '/contacts?$skip=0&$take=3'
    });
    data.resource.items.forEach(function (value) {
        console.log(value);
    });  
});
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "method": "get",
  "uri": "/contacts?$skip=0&$take=3"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.collection+json",
    "resource": {
        "total": 12,
        "itemType": "application/vnd.lime.contact+json",
        "items": [
            {
                "identity": "11121023102013021@messenger.gw.msging.net",
                "name": "John Doe",
                "gender":"male", 
                "group":"friends", 
                "extras":{
                    "plan":"Gold",
                    "code":"1111"
                }
             },
             {
                "identity": "213121@telegram.gw.msging.net",
                "name": "Joseph from Telegram",
                "email":"ze@gmail.com"
             },
             {
                "identity": "5511999990000@take.io",
                "name": "Mary"
             }
        ]
    },
    "method": "get",
    "status": "success",
    "id": "3",
    "from": "postmaster@crm.msging.net/#az-iris5",
    "to": "contact@msging.net",
    "metadata": {
        "#command.uri": "lime://contact@msging.net/contacts?$skip=0&$take=3"
    }
}
```

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Take.Blip.Client.Extensions.Contacts;

namespace Extensions
{
    public class ContactMessageReceiver : IMessageReceiver
    {
        private readonly ISender _sender;
        private readonly Settings _settings;
        private readonly IContactExtension _contactExtension;

        public ContactMessageReceiver(ISender sender, Settings settings, IContactExtension contactExtension)
        {
            _sender = sender;
            _settings = settings;
            _contactExtension = contactExtension;
        }

        public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
        {
            var take = 3;
            var skip = 0;

            var command = new Command{
                Id = EnvelopeId.NewId(),
                Method = CommandMethod.Get,
                Uri = new LimeUri($"/contacts?skip={skip}&$take={take}")
            };

            var commandResponse = await _sender.ProcessCommandAsync(command, cancellationToken);
            var contacts = commandResponse.Resource as DocumentCollection;
        }
    }
}
```

If you need to get more than one chatbot's contact, you can use a query pagination. This sample shows how to take the **three first roaster's contacts**.

| QueryString   | Description                                                                                                                             | <div style="min-width:6em">Example</div> |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| **$skip**     | Number of items to be skipped for paging.                                                                                               | 0                                        |
| **$take**     | Limit of the total of items to be returned. When **not** using filters, values between 1 and 30000 are allowed. When not provided on the request, the default take value will 100. If the used values are not contained on the specified range, thus not allowed, an error will be returned. | 100                                      |
| **$filter**   | Filter to refine a search by contact's properties. When using **$take** and **$skip** along with **$filter** the adition between skip and take values must be lesser or equal to 10000. E.g. skip = 5000 and take = 5000 where the values combined result in 10000.                                                                                     | (startswith(name%2C'John'))              |

<aside class="notice">
Note: Here are some examples about how to filter your query with one of the properties of the contact resource, using the <code>filter</code> property:

<ul>
<li><h4>StartsWith</h4></li>

<code>filter=(startswith({propertyName},'{value}'))</code><br><br>

<b>Example</b>: /contacts?$skip=0&$take=20<b>&$filter=(startswith(name%2C'John'))</b>

<li><h4>Equals</h4></li>

<code>filter=({propertyName} eq '{value}')</code><br><br>
<b>Example</b>: /contacts?$skip=0&$take=20<b>&$filter=(name%20eq%20'John Doe')</b><br><br>
<strong>Not equals</strong> can be used with <code>ne</code>.

<li><h4>Substringof</h4></li>

<i> We are having troubles with <strong>Substringof</strong> performance, we strongly recommend to use <strong>StartsWith</strong> instead.</i><br><br>

<code>filter=(substringof('{value}',{propertyName}))</code><br><br>

<b>Example</b>: /contacts?$skip=0&$take=20<b>&$filter=(substringof('John Doe'%2Cname))</b>

<li><h4>Less than or equal</h4></li>

<code>filter=(lastmessagedate le datetimeoffset'{date}T{hour}:{minute}:{second}.{milisecond}Z')</code><br><br>

<b>Example</b>: /contacts?$skip=0&$take=20<b>&$filter=(lastmessagedate%20le%20datetimeoffset'2021-09-01T13%3A38%3A00.000Z')</b>

<li><h4>Greater than or equal</h4></li>

<code>filter=(lastmessagedate ge datetimeoffset'{date}T{hour}:{minute}:{second}.{milisecond}Z')</code><br><br>

<b>Example</b>: /contacts?$skip=0&$take=20<b>&$filter=(lastmessagedate%20ge%20datetimeoffset'2021-09-01T13%3A38%3A00.000Z')</b>
</ul></aside>

### Send message with contact name

```python
def message_receiver(message: Message) -> None:
    client.sendMessage(
        Message(
            'text/plain',
            'Hello ${contact.name}, welcome to the ${contact.extras.plan} plan!',
            to='11121023102013021@messenger.gw.msging.net',
            metadata={
                '#message.replaceVariables': 'true'
            },
            id='{{$guid}}'
        )
    )

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver))
```

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    await client.sendMessage({  
        id: Lime.Guid(),
        to: '11121023102013021@messenger.gw.msging.net',
        type: 'text/plain',
        content: 'Hello ${contact.name}, welcome to the ${contact.extras.plan} plan!',
        metadata: {
            '#message.replaceVariables': 'true'
        }
    });
});
```

```http
POST https://http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "to": "11121023102013021@messenger.gw.msging.net",
  "type": "text/plain",
  "content": "Hello ${contact.name}, welcome to the ${contact.extras.plan} plan!",
  "metadata": 
  {
    "#message.replaceVariables": "true"
  }
}
```

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Lime.Messaging.Contents;

namespace Extensions
{
    public class PlainTextMessageReceiver : IMessageReceiver
    {
        private readonly ISender _sender;
        private readonly Settings _settings;

        public PlainTextMessageReceiver(ISender sender, Settings settings)
        {
            _sender = sender;
            _settings = settings;
        }

        public async Task ReceiveAsync(Message m, CancellationToken cancellationToken)
        {
            var message = new Message
            {
                To = Node.Parse("11121023102013021@messenger.gw.msging.net"),
                Id = Guid.NewGuid().ToString(),
                Content = new PlainText
                {
                    Text = "Hello ${contact.name}, welcome to the ${contact.extras.plan} plan!"
                },
                Metadata = new Dictionary<string, string>
                {
                    {"#message.replaceVariables", "true" }
                }
            };

            await _sender.SendMessageAsync(message, cancellationToken);
        }
    }
}
```

If you have information of some client stored on the chatbot's roster, you can send a customized message using these values as message variables.
To do this, add a metadata `{ "#message.replaceVariables": "true" }` and use any property of the contact resource. This sample shows how to replace a contact name on a welcome message.
