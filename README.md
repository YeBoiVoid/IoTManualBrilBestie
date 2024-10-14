# Wecome to the manual for the Bril Besties
Bril Besties is a product that battles the lone poopers. With bril besties you are able to talk to your bestie while doing your business.
It uses Arduino's connected to Wi-Fi to form a connection, it makes use of a Telegram bot so can talk with your bestie without a phone.
## What do you need
#### HardWare (Per Person)
- 1x Arduino
- 1x matrix keyboard or compatible full-size keyboard (if you can find it)
- 1x Led strip
#### Software
- Arduino IDE
- AdaFruit NeoPixel
- AdaFruit IO Arduino
- Telegram account
 ### Step 1. Connect modules to the board
 The arduino's cant sent texts or make calls, so we're gonna have to come up with a solution, this solution is a bot in telegram which we can connect our arduino to.
1. Connect the speaker and microphone to the arduino.
2. Connect the light sensor to the board, place it on somewhere where it's clear to the sensor when the toilet is opened.
### Step 2. Set up Telegram to the arduino
1. Make a telegram account, and create a new bot using "BotFather". You can search BotFather, make sure to use the one **WITH** the checkmark.
2. Create a new bot and name it.
3. Connect the bot to where when you type in Telegram, your arduino recieves the message.
### Step 3. Set up AdaFruit IO
1. Create an AdaFruit account.
2. Create a new feed and connnect your arduino to it.
### Step 3. Set up AdaFruit IO with Telegram.
4. Create functions to send and recieve messages to and form the AdaFruit IO.
5. When you send a message to the bot in Telegram, you can read and send the contents through the AdaFruit IO. This is how wer're setting up the "connection" between our arduino's.
6. When you recieve a message make sure you can read the contents, otherwise this system is quite useless.
