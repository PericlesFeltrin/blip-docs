## Native Content

>Sending a Messenger **[text](https://developers.facebook.com/docs/messenger-platform/send-api-reference/)** message:

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Lime.Messaging.Contents;
using Lime.Protocol;
using Take.Blip.Client;

namespace MessageTypes
{
  public class OptionNativeContentReceiver : IMessageReceiver
  {
      private readonly ISender _sender;
      private readonly Settings _settings;

      public OptionNativeContentReceiver(ISender sender)
      {
          _sender = sender;
          _settings = settings;
      }

      public async Task ReceiveAsync(Message message, CancellationToken cancellationToken)
      {
          JsonDocument document = new JsonDocument();
+         document.Add("text", "hello, world!"); //exemplo funcional no messenger

          await _sender.SendMessageAsync(document, message.From, cancellationToken);
      }
  }

}

```

```http
POST https://http.msging.net/messages HTTP/1.1
Content-Type: application/json
Authorization: Key {YOUR_TOKEN}

{
  "id":"1",
  "to":"949839515125748@messenger.gw.msging.net",
  "type":"application/json",
  "content":{
    "text": "hello, world!"
  }
}
```

```python
client.send_message(
  Message.from_json(
    {
      'id':'1',
      'to':'949839515125748@messenger.gw.msging.net',
      'type':'application/json',
      'content':{
        'text': 'hello, world!'
      }
    }
  )
)
```

```javascript
    client.sendMessage({
      id: Lime.Guid(),
      type: "application/vnd.lime.payment-receipt+json",
      to: "128271320123982@messenger.gw.msging.net",
      content: {
          text: "hello, world!"
      }
    });
```

>2 - Sending an **[airline boardingpass template](https://developers.facebook.com/docs/messenger-platform/send-api-reference/airline-boardingpass-template)** message type to Messenger:

```csharp
/*
No examples for C# here
still possible but is too big for this doc
*/
```


```javascript
    client.sendMessage({
      id: Lime.Guid(),
      type: "application/vnd.lime.payment-receipt+json",
      to: "128271320123982@messenger.gw.msging.net",
      content: {
        attachment:{
          type: "template",
          payload:{
            template_type: "airline_boardingpass",
            intro_message: "You are checked in.",
            locale: "en_US",
            boarding_pass:[
              {
                passenger_name: "SMITH\/NICOLAS",
                pnr_number: "CG4X7U",
                travel_class: "business",
                seat: "74J",
                auxiliary_fields:[
                  {
                    label: "Terminal",
                    value: "T1"
                  },
                  {
                    label: "Departure",
                    value: "30OCT 19:05"
                  }
                ],
                secondary_fields:[
                  {
                    label: "Boarding",
                    value: "18:30"
                  },
                  {
                    label: "Gate",
                    value: "D57"
                  },
                  {
                    label: "Seat",
                    value: "74J"
                  },
                  {
                    label: "Sec.Nr.",
                    value: "003"
                  }
                ],
                logo_image_url: "https://www.example.com/en/logo.png",
                header_image_url: "https://www.example.com/en/fb/header.png",
                qr_code: "M1SMITH/NICOLAS  CG4X7U nawouehgawgnapwi3jfa0wfh",
                above_bar_code_image_url: "https://www.example.com/en/PLAT.png",
                flight_info:{
                  flight_number: "KL0642",
                  departure_airport:{
                    airport_code: "JFK",
                    city: "New York",
                    terminal: "T1",
                    gate: "D57"
                  },
                  arrival_airport:{
                    airport_code: "AMS",
                    city: "Amsterdam"
                  },
                  flight_schedule:{
                    departure_time: "2016-01-02T19:05",
                    arrival_time: "2016-01-05T17:30"
                  }
                }
              },
              {
                passenger_name: "JONES/FARBOUND",
                pnr_number: "CG4X7U",
                travel_class: "business",
                seat: "74K",
                auxiliary_fields:[
                  {
                    label: "Terminal",
                    value: "T1"
                  },
                  {
                    label: "Departure",
                    value: "30OCT 19:05"
                  }
                ],
                secondary_fields:[
                  {
                    label: "Boarding",
                    value: "18:30"
                  },
                  {
                    label: "Gate",
                    value: "D57"
                  },
                  {
                    label: "Seat",
                    value: "74K"
                  },
                  {
                    label: "Sec.Nr.",
                    value: "004"
                  }
                ],
                logo_image_url: "https://www.example.com/en/logo.png",
                header_image_url: "https://www.example.com/en/fb/header.png",
                qr_code: "M1JONES/FARBOUND  CG4X7U nawouehgawgnapwi3jfa0wfh",
                above_bar_code_image_url: "https://www.example.com/en/PLAT.png",
                flight_info:{
                  flight_number: "KL0642",
                  departure_airport:{
                    airport_code: "JFK",
                    city: "New York",
                    terminal: "T1",
                    gate: "D57"
                  },
                  arrival_airport:{
                    airport_code: "AMS",
                    city: "Amsterdam"
                  },
                  flight_schedule:{
                    departure_time: "2016-01-02T19:05",
                    arrival_time: "2016-01-05T17:30"
                  }
                }
              }
            ]
          }
        }
      }
    });
```

```python
client.send_message(
  Message.from_json(
    {
      'id':'2',
      'to':'949839515125748@messenger.gw.msging.net',
      'type':'application/json',
      'content':{
        'attachment':{
          'type':'template',
          'payload':{
            'template_type':'airline_boardingpass',
            'intro_message':'You are checked in.',
            'locale':'en_US',
            'boarding_pass':[
              {
                'passenger_name':'SMITH\/NICOLAS',
                'pnr_number':'CG4X7U',
                'travel_class':'business',
                'seat':'74J',
                'auxiliary_fields':[
                  {
                    'label':'Terminal',
                    'value':'T1'
                  },
                  {
                    'label':'Departure',
                    'value':'30OCT 19:05'
                  }
                ],
                'secondary_fields':[
                  {
                    'label':'Boarding',
                    'value':'18:30'
                  },
                  {
                    'label':'Gate',
                    'value':'D57'
                  },
                  {
                    'label':'Seat',
                    'value':'74J'
                  },
                  {
                    'label':'Sec.Nr.',
                    'value':'003'
                  }
                ],
                'logo_image_url':'https://www.example.com/en/logo.png',
                'header_image_url':'https://www.example.com/en/fb/header.png',
                'qr_code':'M1SMITH/NICOLAS  CG4X7U nawouehgawgnapwi3jfa0wfh',
                'above_bar_code_image_url':'https://www.example.com/en/PLAT.png',
                'flight_info':{
                  'flight_number':'KL0642',
                  'departure_airport':{
                    'airport_code':'JFK',
                    'city':'New York',
                    'terminal':'T1',
                    'gate':'D57'
                  },
                  'arrival_airport':{
                    'airport_code':'AMS',
                    'city':'Amsterdam'
                  },
                  'flight_schedule':{
                    'departure_time':'2016-01-02T19:05',
                    'arrival_time':'2016-01-05T17:30'
                  }
                }
              },
              {
                'passenger_name':'JONES/FARBOUND',
                'pnr_number':'CG4X7U',
                'travel_class':'business',
                'seat':'74K',
                'auxiliary_fields':[
                  {
                    'label':'Terminal',
                    'value':'T1'
                  },
                  {
                    'label':'Departure',
                    'value':'30OCT 19:05'
                  }
                ],
                'secondary_fields':[
                  {
                    'label':'Boarding',
                    'value':'18:30'
                  },
                  {
                    'label':'Gate',
                    'value':'D57'
                  },
                  {
                    'label':'Seat',
                    'value':'74K'
                  },
                  {
                    'label':'Sec.Nr.',
                    'value':'004'
                  }
                ],
                'logo_image_url':'https://www.example.com/en/logo.png',
                'header_image_url':'https://www.example.com/en/fb/header.png',
                'qr_code':'M1JONES/FARBOUND  CG4X7U nawouehgawgnapwi3jfa0wfh',
                'above_bar_code_image_url':'https://www.example.com/en/PLAT.png',
                'flight_info':{
                  'flight_number':'KL0642',
                  'departure_airport':{
                    'airport_code':'JFK',
                    'city':'New York',
                    'terminal':'T1',
                    'gate':'D57'
                  },
                  'arrival_airport':{
                    'airport_code':'AMS',
                    'city':'Amsterdam'
                  },
                  'flight_schedule':{
                    'departure_time':'2016-01-02T19:05',
                    'arrival_time':'2016-01-05T17:30'
                  }
                }
              }
            ]
          }
        }
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
  "id":"2",
  "to":"949839515125748@messenger.gw.msging.net",
  "type":"application/json",
  "content":{
    "attachment":{
      "type":"template",
      "payload":{
        "template_type":"airline_boardingpass",
        "intro_message":"You are checked in.",
        "locale":"en_US",
        "boarding_pass":[
          {
            "passenger_name":"SMITH\/NICOLAS",
            "pnr_number":"CG4X7U",
            "travel_class":"business",
            "seat":"74J",
            "auxiliary_fields":[
              {
                "label":"Terminal",
                "value":"T1"
              },
              {
                "label":"Departure",
                "value":"30OCT 19:05"
              }
            ],
            "secondary_fields":[
              {
                "label":"Boarding",
                "value":"18:30"
              },
              {
                "label":"Gate",
                "value":"D57"
              },
              {
                "label":"Seat",
                "value":"74J"
              },
              {
                "label":"Sec.Nr.",
                "value":"003"
              }
            ],
            "logo_image_url":"https://www.example.com/en/logo.png",
            "header_image_url":"https://www.example.com/en/fb/header.png",
            "qr_code":"M1SMITH/NICOLAS  CG4X7U nawouehgawgnapwi3jfa0wfh",
            "above_bar_code_image_url":"https://www.example.com/en/PLAT.png",
            "flight_info":{
              "flight_number":"KL0642",
              "departure_airport":{
                "airport_code":"JFK",
                "city":"New York",
                "terminal":"T1",
                "gate":"D57"
              },
              "arrival_airport":{
                "airport_code":"AMS",
                "city":"Amsterdam"
              },
              "flight_schedule":{
                "departure_time":"2016-01-02T19:05",
                "arrival_time":"2016-01-05T17:30"
              }
            }
          },
          {
            "passenger_name":"JONES/FARBOUND",
            "pnr_number":"CG4X7U",
            "travel_class":"business",
            "seat":"74K",
            "auxiliary_fields":[
              {
                "label":"Terminal",
                "value":"T1"
              },
              {
                "label":"Departure",
                "value":"30OCT 19:05"
              }
            ],
            "secondary_fields":[
              {
                "label":"Boarding",
                "value":"18:30"
              },
              {
                "label":"Gate",
                "value":"D57"
              },
              {
                "label":"Seat",
                "value":"74K"
              },
              {
                "label":"Sec.Nr.",
                "value":"004"
              }
            ],
            "logo_image_url":"https://www.example.com/en/logo.png",
            "header_image_url":"https://www.example.com/en/fb/header.png",
            "qr_code":"M1JONES/FARBOUND  CG4X7U nawouehgawgnapwi3jfa0wfh",
            "above_bar_code_image_url":"https://www.example.com/en/PLAT.png",
            "flight_info":{
              "flight_number":"KL0642",
              "departure_airport":{
                "airport_code":"JFK",
                "city":"New York",
                "terminal":"T1",
                "gate":"D57"
              },
              "arrival_airport":{
                "airport_code":"AMS",
                "city":"Amsterdam"
              },
              "flight_schedule":{
                "departure_time":"2016-01-02T19:05",
                "arrival_time":"2016-01-05T17:30"
              }
            }
          }
        ]
      }
    }
  }
}
```


| MIME type        |
|------------------|
| application/json |

Allows sending of a native content of some channel using JSON format. It is possible to use any channel's available resource, even if this content is not yet supported as a Blip canonical type.

Note that, for a **multi channel** chatbot, it is the chatbot developer's responsibility to send the correct content type to each channel.

#### Channel mapping

| Channel   | Type                                                                                                                                                               |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Blip App  | Does not support                                                                                                                                                   |
| Messenger | Supported (the property `content` refers to `message` element of Messenger [Send API](https://developers.facebook.com/docs/messenger-platform/send-api-reference/) |
| Whatsapp  | Does not support                                                                                                                                                   |
| SMS       | Does not support                                                                                                                                                   |
| Skype     | Does not support                                                                                                                                                   |
| Telegram  | Does not support                                                                                                                                                   |
