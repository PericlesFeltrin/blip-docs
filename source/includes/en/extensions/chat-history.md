## Chat history

The **threads** (or **chat history**) extension allows the chatbot to retrieve the last threads and messages exchanged with the customers.

To get client's **threads** or **messages** exchanged with a bot, send a command with the following properties:

| Name   | Description                              |
|--------|------------------------------------------|
| id     | Unique identifier of the command.        |
| method | **get**                                  |
| uri    | **/threads**                             |
| to     | **postmaster@msging.net** (not required) |

<aside class="notice">Note: To have the server automatically refresh any expired `MediaLink` messages returned by `/threads` and retrieve a valid URL, pass the <b>refreshExpiredMedia=true</b> parameter into the query string.
</aside>

<!-- ### Get average response time

Get the average [messages](/#messages) response time. This will return a [ResponseTime](/#responsetime) document.

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "131237885",
  "to": "postmaster@msging.net",
  "method": "get",
  "uri": "/message-statistic/responseTime"
}
``` -->

### Get last threads

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    var lastThreads = await client.sendCommand({  
        id: Lime.Guid(),
        method: Lime.CommandMethod.GET,
        uri: '/threads?refreshExpiredMedia=true'
    });
    lastThreads.resource.items.forEach(function (item) {
        console.log(item);
    });  
});
```

```python
async def message_receiver_async(message: Message) -> None:
    result = await client.chat_extension.get_threads_async(refresh_expired_media=True)

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver_async))
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "method": "get",
  "uri": "/threads?refreshExpiredMedia=true"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "type": "application/vnd.lime.collection+json",
  "resource": {
    "itemType": "application/vnd.iris.thread+json",
    "items": [
      {
        "ownerIdentity": "contact@msging.net",
        "identity": "1180740631991418@messenger.gw.msging.net",
        "lastMessage": {
          "id": "39ed84b9-f89e-4090-a27e-6bd1e69bdfef",
          "direction": "sent",
          "type": "text/plain",
          "content": "Welcome to our service!",
          "date": "2016-12-06T12:32:44.799Z"
        },
        "unreadMessages": 0
      },
      {
        "ownerIdentity": "contact@msging.net",
        "identity": "29%3A1SaTsDWumQFx72srIXI8uhTlpRzPwuJ4TRVhRpSBB7mQ@skype.gw.msging.net",
        "lastMessage": {
          "id": "cc2b70ce-921b-4856-ae41-f00d897f1423",
          "direction": "received",
          "type": "text/plain",
          "content": "Hi",
          "date": "2016-11-24T20:41:38.940Z"
        },
        "unreadMessages": 1
      }
    ]
  },
  "method": "get",
  "status": "success",
  "id": "0094447a-2581-4597-be6a-a5dff33af156",
  "from": "postmaster@msging.net/#az-iris3",
  "to": "contact@msging.net",
  "metadata": {
      "#command.uri": "lime://contact@msging.net/threads"
  }
}
```

```csharp
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Take.Blip.Client.Extensions.Threads;

namespace Extensions
{
    public class GetThreadsReceiver : IMessageReceiver
    {
        private readonly IThreadExtension _threadExtension;

        public GetThreadsReceiver(IThreadExtension threadExtension)
        {
            _threadExtension = threadExtension;
        }

        public async Task ReceiveAsync(Message receivedMessage, CancellationToken cancellationToken)
        {
            var lastThreads = await _threadExtension.GetThreadsAsync(cancellationToken);
        }
    }
}
```

Getting the last chatbot's [threads](/#thread). By default, Blip returns the last 50 threads.

The following uri filters are available to get chatbot's threads:

| QueryString         | Description                                                              |
|---------------------|--------------------------------------------------------------------------|
| $take               | Limit of total of items to be returned. The maximum value allowed is 100 |
| $skip               | The number of elements to be skipped                                     |
| messageDate         | Initial date on the threads query                                        |
| refreshExpiredMedia | Defines if the expired media links should be refreshed                   |

### Get last messages

```javascript
client.addMessageReceiver('text/plain', async (message) => {
    var lastMessages = await client.sendCommand({  
        id: Lime.Guid(),
        method: Lime.CommandMethod.GET,
        uri: '/threads/{{user_identity}}?refreshExpiredMedia=true'
    });
    lastMessages.resource.items.forEach(function (item) {
        console.log(item);
    });  
});
```

```python
async def message_receiver_async(message: Message) -> None:
    result = await client.chat_extension.get_thread_async(
        '{{user_identity}}',
        refresh_expired_media=True
    )

client.add_message_receiver(Receiver(lambda m: m.type_n == 'text/plain', message_receiver_async))
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{  
  "id": "{{$guid}}",
  "method": "get",
  "uri": "/threads/{{user_identity}}?refreshExpiredMedia=true"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "type": "application/vnd.lime.collection+json",
  "resource": {
    "total": 3,
    "itemType": "application/vnd.iris.thread-message+json",
    "items": [
      {
        "id": "39ed84b9-f89e-4090-a27e-6bd1e69bdfef",
        "direction": "sent",
        "type": "text/plain",
        "content": "Welcome!",
        "date": "2016-12-06T12:32:44.799Z",
        "status": "consumed"
      },
      {
        "id": "15073ef5-9bab-493c-b630-8636eacba33e",
        "direction": "sent",
        "type": "text/plain",
        "content": "This is a test chatbot.",
        "date": "2016-12-06T12:32:40.640Z",
        "status": "consumed"
      },
      {
        "id": "9b49a7d6-d025-4bb6-a370-1d48fb457deb",
        "direction": "received",
        "type": "text/plain",
        "content": "Good morning",
        "date": "2016-12-06T12:32:35.398Z",
        "status": "accepted"
      }
    ]
  },
  "method": "get",
  "status": "success",
  "id": "0094447a-2581-4597-be6a-a5dff33af156",
  "from": "postmaster@msging.net/#az-iris1",
  "to": "contact@msging.net",
  "metadata": {
      "#command.uri": "lime://contact@msging.net/threads/destination@0mn.io"
  }
}
```

```csharp
using System.Threading;
using System.Threading.Tasks;
using Lime.Protocol;
using Take.Blip.Client;
using Take.Blip.Client.Extensions.Threads;

namespace Extensions
{
    public class GetMessagesReceiver : IMessageReceiver
    {
        private readonly IThreadExtension _threadExtension;

        public GetMessagesReceiver(IThreadExtension threadExtension)
        {
            _threadExtension = threadExtension;
        }

        public async Task ReceiveAsync(Message receivedMessage, CancellationToken cancellationToken)
        {
            var lastMessages = await _threadExtension.GetThreadAsync(
                "destination@0mn.io", 
                cancellationToken
            );
        }
    }
}
```

Getting the last chatbot's [messages](/#messages) in a specific [thread](/#thread). The thread is identified by a client identity (for example: `1180740631991418@messenger.gw.msging.net`). By default, Blip will return the last 20 thread messages.

The following uri filters are available to get a chatbot's thread:

| QueryString         | Description                                                                                                                                                                                                            |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| $skip               | The number of elements to be skipped                                                                                                                                                                                   |
| $take               | Limit of total of items to be returned. The maximum value allowed is 100                                                                                                                                               |
| messageId           | Initial message id for the thread messages query                                                                                                                                                                       |
| storageDate         | The reference date to search. Example: `2020-01-08T15:59:07.086Z`                                                                                                                                                      |
| direction           | Possible values: `asc` and `desc`. Define whether messages will be returned after(in ascending order) or before(in descending order) a date, respectively. <i>Needs **storageDate** or **messageId** to be defined</i> |
| refreshExpiredMedia | Defines if the expired media links should be refreshed                                                                                                                                                                 |

<aside class="notice">
Note: Both <b>storageDate</b> filter and <b>date</b> response parameter uses <b>ISO 8601</b> format. However, the return information is always at <b>GMT 00:00</b>.
</aside>

<aside class="notice">
Note: The <b>storageDate</b> filter can be passed both as UTC or GMT-03 timezones.<br><b>Example:</b><br>2020-06-18T11:30:44.000Z => UTC<br>2020-06-18T11:30:44.000-03:00 => GMT -03
</aside>

<aside class="notice">
If you want to paginate your query to get more than 100 results, you must use the <b>storageDate</b> and <b>direction</b> filters.<br><b>Example:</b><br>/threads/{userIdentity}?storageDate=2020-05-29T00:00:00.531Z&direction=desc
</aside>

### Get logged messages

Get all logged [messages](/#messages). By default, Blip returns the last 100 logged messages.

| QueryString | Description                                                              |
|-------------|--------------------------------------------------------------------------|
| $skip       | The number of elements to be skipped                                     |
| $take       | Limit of total of items to be returned. The maximum value allowed is 100 |

```python
result = await client.process_command_async(
    Command.from_json(
        {
            'id': '{{$guid}}',
            'to': 'postmaster@msging.net',
            'method': 'get',
            'uri': '/messages'
        }
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@msging.net",
  "method": "get",
  "uri": "/messages"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.collection+json",
    "resource": {
        "total": 2,
        "itemType": "application/vnd.lime.message+json",
        "items": [
            {
                "type": "text/plain",
                "content": "Example",
                "id": "b2fea46b-a006-49e2-82fc-2e61e7832757",
                "from": "demobot@msging.net",
                "pp": "distList/demobot%40msging.net",
                "to": "2624285677641447@messenger.gw.msging.net",
                "metadata": {
                    "#messagePurpose": "CONFIRMED_EVENT_REMINDER",
                    "#scheduler.when": "11/26/2019 18:21:22",
                    "#message.replaceVariables": "True",
                    "#messageKind": "Active",
                    "#channel.preserveClaims": "True",
                    "#envelope.storageDate": "2019-11-26T18:21:27.000Z"
                }
            },
            {
                "type": "text/plain",
                "content": "Nice!!",
                "id": "b2fea46b-a006-49e2-82fc-2e61e7832757",
                "from": "demobot@msging.net",
                "pp": "postmaster/demobot%40msging.net",
                "to": "distList@broadcast.msging.net",
                "metadata": {
                    "#messagePurpose": "CONFIRMED_EVENT_REMINDER",
                    "#scheduler.when": "11/26/2019 18:21:22",
                    "#envelope.storageDate": "2019-11-26T18:21:27.000Z"
                }
            }
        ]
    },
    "method": "get",
    "status": "success",
    "id": "1b411af6-5057-4e9a-bd7e-4ec70b253b7e",
    "from": "postmaster@msging.net/#az-iris6",
    "to": "demobo4@msging.net",
}
```

### Get logged notifications

Get all logged [notifications](/#notifications). By default, Blip returns the last 100 logged notifications.

| QueryString | Description                                                              |
|-------------|--------------------------------------------------------------------------|
| id          | The notification id to filter                                            |
| $skip       | The number of elements to be skipped                                     |
| $take       | Limit of total of items to be returned. The maximum value allowed is 100 |

<aside class="notice">
Note: You can get notifications of a message after a <b>maximum period of 3 days from the current date</b>. All message notifications after that date are inaccessible and will not bet returned.
</aside>

```python
result = await client.process_command_async(
    Command.from_json(
        {
            'id': '{{$guid}}',
            'to': 'postmaster@msging.net',
            'method': 'get',
            'uri': '/notifications'
        }
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@msging.net",
  "method": "get",
  "uri": "/notifications"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.collection+json",
    "resource": {
        "total": 4,
        "itemType": "application/vnd.lime.notification+json",
        "items": [
            {
                "event": "dispatched",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "d8ae95f8-3dfb9b@tunnel.msging.net",
                "to": "demobot4@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680445",
                    "#message.to": "routertestetelegram@msging.net",
                    "#tunnel.owner": "postmaster@msging.net",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "dispatched",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "25bcf24d-f34b51ed4@tunnel.msging.net",
                "to": "demobot@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680461",
                    "#message.to": "routertestetelegram@msging.net",
                    "#tunnel.owner": "master.hosting@msging.net",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "accepted",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "25bcf24d-f4b51ed4@tunnel.msging.net",
                "to": "demobot@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680456",
                    "#message.to": "routertestetelegram@msging.net",
                    "#tunnel.owner": "master.hosting@msging.net",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "received",
                "id": "fwd:75d5fce4-91fc-40be-bd61-b1e1b99a3c64",
                "from": "postmaster@msging.net/#az-iris1",
                "to": "demobot@msging.net/msging-applicationjgp",
                "metadata": {
                    "#envelope.timestamp": "1574786308966",
                    "#envelope.storageDate": "2019-11-26T16:38:28.000Z"
                }
            }
        ]
    },
    "method": "get",
    "status": "success",
    "id": "45321221-19ea-45f9-97d5-78f20876cb33",
    "from": "postmaster@msging.net/#az-iris6",
    "to": "demobot4@msging.net"
}
```
### Get notifications of a message

Get all logged [notifications](#notifications) of a specific [message](/#messages).

Replace `{messageId}` with the message id.

```python
result = await client.process_command_async(
    Command.from_json(
        {
            'id': '{{$guid}}',
            'to': 'postmaster@msging.net',
            'method': 'get',
            'uri': '/notifications?id={messageId}'
        }
    )
)
```

```http
POST https://http.msging.net/commands HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id": "{{$guid}}",
  "to": "postmaster@msging.net",
  "method": "get",
  "uri": "/notifications?id={messageId}"
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "type": "application/vnd.lime.collection+json",
    "resource": {
        "total": 4,
        "itemType": "application/vnd.lime.notification+json",
        "items": [
            {
                "event": "dispatched",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "d8adfb9b@tunnel.msging.net",
                "to": "demobot@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680445",
                    "#tunnel.owner": "postmaster@msging.net",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "dispatched",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "25b4b51ed4@tunnel.msging.net",
                "to": "demobot@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680461",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "accepted",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "25bc1ed4@tunnel.msging.net",
                "to": "demobot4@msging.net",
                "metadata": {
                    "#envelope.timestamp": "1574943680456",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            },
            {
                "event": "accepted",
                "id": "fffc5c3c-a31e-43d6-b6a6-319f250f5248",
                "from": "postmaster@msging.net/#az-iris6",
                "to": "demobot@msging.net/msging-application-builderwqfh",
                "metadata": {
                    "#envelope.timestamp": "1574943680410",
                    "#message.to": "cb37a4@tunnel.msging.net",
                    "#envelope.storageDate": "2019-11-28T12:21:20.000Z"
                }
            }
        ]
    },
    "method": "get",
    "status": "success",
    "id": "8602b1e6-dc76-45c6-a0ef-04c7a1050fed",
    "from": "postmaster@msging.net/#az-iris6",
    "to": "demobot4@msging.net"
}
```
