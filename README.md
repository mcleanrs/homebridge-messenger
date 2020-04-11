<p align="center">
    <img src="https://user-images.githubusercontent.com/1062160/79011991-45a1c680-7b33-11ea-9e34-ec5e9005f623.png" height="120"><br>
    <img src="https://user-images.githubusercontent.com/1062160/79012099-7da90980-7b33-11ea-99a3-c69c322e875e.png" hspace="10px">
    <img src="https://user-images.githubusercontent.com/1062160/79012203-c06ae180-7b33-11ea-81c3-ab43290011db.png" hspace="10px">    
</p>

<span align="center">
    
# homebridge-messenger
[![NPM version](https://img.shields.io/npm/v/homebridge-messenger?color=blue)](https://npmjs.com/package/homebridge-messenger)
[![GitHub issues](https://img.shields.io/github/issues/potrudeau/homebridge-messenger)](https://github.com/potrudeau/homebridge-messenger/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/potrudeau/homebridge-messenger)](https://github.com/potrudeau/homebridge-messenger/pulls)
[![Licence](https://img.shields.io/npm/l/homebridge-messenger?color=red)](LICENSE)

[Homebridge](http://homebridge.io) plugin which allow users to send messages from [HomeKit](https://developer.apple.com/homekit/).

</span>

## Features
The plugin supports the following technologies:
* [Pushover](https://pushover.net/)
* [SMTP email](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)

## Installation

**Option 1: Install via Homebridge Config UI X:**

Search for "messenger" in [homebridge-config-ui-x](https://github.com/oznu/homebridge-config-ui-x) and install `homebridge-messenger`.

**Option 2: Manually Install:**

```
sudo npm install -g homebridge-messenger
```

## Configuration
The plugin adds to HomeKit a main switch and switches for each configured message. 

The accessory **name** is used to specify the name of the main switch. If this main switch if turned OFF, no message will be sent by the plugin.

Message switches are stateful: they are turned OFF after being turned ON, once the message is sent.

There are two levels of configuration:
* **Service** level properties defines the parameters for a specific technology. All messages related to that technology will be using the same properties for all messages. 
    * Example: All emails will use the same SMTP configuration. 
* **Message** level properties contains the configuration of each message. 
    * Example: Each message can have a different title and text.
    
All switches can be used in scenes and automation.

### Homebridge Config UI X
*Coming soon...*
### Pushover
```
"accessories": [
      {
          "accessory": "HomebridgeMessenger",
          "name": "Messenger",
          "services": {
              "pushover": {
                  "user": "your_user",
                  "token": "your_token"
              }
          },
          "messages": [
              {
                  "type": "pushover",
                  "name": "Pushover message",
                  "text": "This is a test",
                  "sound": "pushover",
                  "priority": 0
              },
              {
                  "type": "pushover",
                  "name": "Critical message",
                  "text": "This is important",
                  "sound": "magic",
                  "priority": 2
              }         
          ]
      }
  ]
```
* Service properties : 
    * user *(required)*: The user/group key of your user (or you).
    * token *(required)*: Your application's API token.
* Message properties :
    * type *(required)*: Must be `pushover`.
    * name *(required)*: Name of the switch and title of your message.
    * text *(required)*: Body of the message.
    * sound *(facultative)*: Name of the sound that will notify the user. If no valid value is provided, the default `pushover` sound will be used. For no sound, use `silent`, the [Pushover API](https://pushover.net/api#sounds) contains the list of other available sounds.
    * priority *(facultative)*: Priority of the message. Accepted values are `-2`, `-1`, `0`, `1` or `2`. You may refer to the [Pushover API](https://pushover.net/api#priority) for more details. Critical messages (`2`), are sent with the following parameters :
        * Retry : 60 seconds
        * Expires: 3600 seconds
    
### Email
```
"accessories": [
    {
        "accessory": "HomebridgeMessenger",
        "name": "Messenger",
        "services": {
            "email": {
                "recipient": "you_email@domain.com",
                "smtpServer": "smtp.domain.com",
                "smtpPort": 465,
                "smtpSecure": true,
                "smtpUsername": "your_username",
                "smtpPassword": "your_password"
            }
        },
        "messages": [
            {
                "type": "email",
                "name": "Test email",
                "text": "This is a test"
            }        
        ]
    }
]
```
* Service properties : 
    * recipient *(required)*: Email address of the recipient.
    * smtpServer *(required)*: Address of the SMTP host.
    * smtpPort *(facultative)*: Port to connect to. (Default value is `25`).
    * smtpSecure *(facultative)*: Set to `true` if SMTP supports TLS. (Default value is `false`).
    * smtpUsername *(facultative)*: Username for the SMTP server, if required.
    * smtpPassword *(facultative)*: Password for the SMTP server, if required.
* Message properties :
    * type *(required)*: Must be `email`.
    * name *(required)*: Name of the switch and subject of your email.
    * text *(required)*: Body of the email.
    
### Advanced configuration
An example featuring **all services** is available [here](configuration-examples/advanced.example.json).

## Coming next
* Configuration using [homebridge-config-ui-x](https://github.com/oznu/homebridge-config-ui-x)
* Add plugin cache for status.
* Support for [Pushbullet](https://www.pushbullet.com)
* Support for [IFTTT](https://ifttt.com)
* Improved logging

## Credits
* [qbit/node-pushover](https://github.com/qbit/node-pushover) - library to send Pushover messages
* [nodemailer](https://github.com/nodemailer/nodemailer) - library to send SMTP emails

## License
The [homebridge-messenger](https://github.com/potrudeau/homebridge-messenger) plugin is released under the [MIT license](LICENSE).
