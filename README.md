# IFMA
 
### IFMA (Images from Mail Attachments) is a Node-Red Flow that allows you to get images from mail attachments from cheap IP cameras and make those usable for Home Automation or anything else.
<img src="images/IFMA-flow.png?raw=true">

## Usecases
1) Forward all your cameras default notifications to your Telegram Bot.
2) Save all the images produced from your camera motion sensor into a directory without the need to have an SD-card fisically mounted on the camera itself.
3) Create a "Last Motion" tab in your Home automation software like Homeassistant.
4) Create a .GIF from a bounch of images 
5) Anything else you can imagine.

## Setup - How to get the Images
### Your camera should allows you tu send a image via mail attachment when motion is detected.
### You have to enable and configure this option.
### Make sure to have Node-Red installed.
### Make sure to have the "Mail" node (you should have it by default under the Social tab)
<img src="images/mail-node.png?raw=true">

### Copy or import the flow configuration from <a href="Node-Red/IFMA.json">here</a>:

```bash
[
    {
        "id": "b96a06b7.5821c8",
        "type": "tab",
        "label": "Flow 1",
        "disabled": false,
        "info": ""
    },
    {
        "id": "b7274475.b95da8",
        "type": "e-mail in",
        "z": "b96a06b7.5821c8",
        "name": "Check new mail",
        "protocol": "IMAP",
        "server": "imap.gmail.com",
        "useSSL": true,
        "port": "993",
        "box": "INBOX",
        "disposition": "Read",
        "criteria": "UNSEEN",
        "repeat": "30",
        "fetch": "auto",
        "inputs": 0,
        "x": 120,
        "y": 60,
        "wires": [
            [
                "467a9891.294018"
            ]
        ]
    },
    {
        "id": "467a9891.294018",
        "type": "change",
        "z": "b96a06b7.5821c8",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "attachments",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 320,
        "y": 60,
        "wires": [
            [
                "8cdc3efb.7659"
            ]
        ]
    },
    {
        "id": "8cdc3efb.7659",
        "type": "split",
        "z": "b96a06b7.5821c8",
        "name": "",
        "splt": "\\n",
        "spltType": "str",
        "arraySplt": 1,
        "arraySpltType": "len",
        "stream": false,
        "addname": "",
        "x": 510,
        "y": 60,
        "wires": [
            [
                "33d0c5f3.b83d0a"
            ]
        ]
    },
    {
        "id": "33d0c5f3.b83d0a",
        "type": "change",
        "z": "b96a06b7.5821c8",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "filename",
                "pt": "msg",
                "to": "$join([\"/config/www/\", payload.fileName])",
                "tot": "jsonata"
            },
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.content",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 680,
        "y": 60,
        "wires": [
            [
                "ecec80a7.d35bc",
                "cbad2429.567088"
            ]
        ]
    },
    {
        "id": "ecec80a7.d35bc",
        "type": "image",
        "z": "b96a06b7.5821c8",
        "name": "",
        "width": 160,
        "x": 680,
        "y": 100,
        "wires": []
    },
    {
        "id": "cbad2429.567088",
        "type": "file",
        "z": "b96a06b7.5821c8",
        "name": "",
        "filename": "/config/www/image.jpg",
        "appendNewline": true,
        "createDir": false,
        "overwriteFile": "true",
        "encoding": "none",
        "x": 910,
        "y": 60,
        "wires": [
            [
                "f3c3e51.ebd6518"
            ]
        ]
    },
    {
        "id": "f3c3e51.ebd6518",
        "type": "image",
        "z": "b96a06b7.5821c8",
        "name": "",
        "width": 160,
        "x": 940,
        "y": 100,
        "wires": []
    }
]
```

### Configure the Mail input node
<img src="images/config-example.png?raw=true">

### Configure your directory in the "Change" Node and in the "File Output" Node  


