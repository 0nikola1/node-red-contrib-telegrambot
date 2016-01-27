# Telegram bot nodes for node-red

This package contains a receiver and a sender node which act as a telegram bot.
The only thing required is the token that can be retrieved by the @botfather telegram bot.
https://core.telegram.org/bots

The nodes are a simple wrapper around the  [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api)

# Usage
The input node receives messages from the bot and sends a message object with the following layout:
- **payload** contains the message details
  - **chatId**  : the unique id of the chat. This value needs to be passed to the out node when responding to the same chat.
  - **type**    : the type of message received: message, photo, audio, location, video, voice, contact
  - **content** : received message content: string or file_id, or object with full data (location, contact)

- **originalMessage** contains the original message object from the underlying [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api) lib.


The simple echo flow looks like:
![Alt text](images/TelegramBotEcho.png?raw=true "Echo Flow")


## Configuration Node
The only thing to be entered here is the token which you received from @botfather when creating a new bot.
The string is automatically trimmed.

## Receiver Node
This node receives all messages from a chat. Simply invite the bot to a chat. 
(You can control if the bot receives every message by calling /setprivacy @botfather.)
The original message from the underlying node library is stored in msg.originalMessage.
msg.payload contains the most important data like chatId, type and content. The content depends
on the message type. E.g. if you receive a message then the content is a string. If you receive a location,
then the content is an object containing latitude and logitude. 

## Sender Node
This node sends the payload to the chat. The payload must contain the floowing fields:
msg.payload.chatId  - chatId
msg.payload.type    - e.g. "message"
msg.payload.content - your message text

## Command Node
The command node can be used for triggering a message when a specified command is received: e.g. help.
See example below.
It has two outputs
- 1. is triggered when the command is received
- 2. is triggered when the command is not received

The second one is userful when you want to use a keyboard. 
See example below.

## Implementing a simple echo 
![Alt text](images/TelegramBotEcho.png?raw=true "Echo Flow")
This example is self-explaining. The received message is returned to the sender.

## Implementing a /help command
![Alt text](images/TelegramBotHelp.png?raw=true "Help Command Flow")
This flow returns the help message of your bot. It receives the command and creates a new message,
which is returned:
![Alt text](images/TelegramBotHelp2.png?raw=true "Help Function")
Note: You can access the sender's data via the originalMessage property.

## Implementing a keyboard
![Alt text](images/TelegramBotConfirmationMessage.png?raw=true "Keyboard Flow")
Keyboards are very useful for getting additional data from the sender.
When the command is received the first output is triggered and a dialog is opened:
![Alt text](images/TelegramBotConfirmationMessage2.png?raw=true "Keyboard Function 1")
The answer is send to the second output triggering the lower flow. Data is passed via global properties here.
![Alt text](images/TelegramBotConfirmationMessage3.png?raw=true "Keyboard Function 2")
 
## Receiving a location
Locations can be send to the chat. The bot can receive the longitude and latitude:
![Alt text](images/TelegramBotLocation.png?raw=true "Location Function")

 ## Sending messages to a specified chat 
![Alt text](images/TelegramBotSendToChat.png?raw=true "Sending a message")
If you have the chatId, you can send any message without the need of having received something before.

## Implementing a simple bot 
![Alt text](images/TelegramBotExample.png?raw=true "Bot example")
Putting all pieces together you will have a simple bot implementing some userful functions.

All example flows can be found in the examples folder of this package. 

# Warning
The project is under heavy construction right now.

Author: Karl-Heinz Wind

The MIT License (MIT)
Copyright (c) <year> <copyright holders>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.