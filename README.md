# Wecome to the manual for the Bril Besties
Bril Besties is a product that battles the lone poopers. With bril besties you are able to talk to your bestie while doing your business.
It uses Arduino's connected to Wi-Fi to form a connection, it makes use of a Telegram bot so can talk with your bestie without a phone.
## What do you need
#### HardWare (Per Person)
- 1x Arduino
#### Software
- Arduino IDE
- AdaFruit NeoPixel
- AdaFruit IO Arduino
- Telegram (you can use the web version)
## Actually conencting everything together
Now is the time we connect everything together. 
 ### Step 1. Connect modules to the board
 The arduino's cant sent texts or make calls, this makes connecting the modules qutie easy as there are no modules to connect to. We only have to connect two things to the arduino, these are a bot on Telegram and the AdaFruit IO website. We'll go over this in the coming steps.
### Step 2. Set up Telegram to the arduino
1. Make a telegram account.
2. You can search BotFather, make sure to use the one **WITH** the checkmark.
   ![BotFather Account] (/BotFather.png)
4. Create a new bot and name it.
5. Connect the bot to where when you type in Telegram, your arduino recieves the message.

### Step 3. Set up AdaFruit IO
1. Create an AdaFruit account.
2. Create a new feed and connnect your arduino to it.

### Step 3. Set up AdaFruit IO with Telegram.
Since voice messages will not be doable, text will have to do. The AdaFruit IO cannot send words. This is quite troubling if you want to send messages. However, it can send numeric values. And the alphabeth exists, so all we need to do is create an function that converts words into numeric values. So a word like "hello" would be "8 5 9 9 15", these values are something we can send to the AdaFruit IO. 
1. Create functions that convert sentences into numeric values. I did this within two functions so my code doesnt turn into spaghetti:
    
3. When you send a message to the bot in Telegram, you can read and send the contents through the AdaFruit IO. This is how wer're setting up the "connection" between our arduino's.
4. When you recieve a message make sure you can read the contents, otherwise this system is quite useless.

### Step 4. Trouble Shooting with the led strip.
Our led strip is a great way of trouble shooting our system, since we dont always have acces to the serial monitor our strip will act as one.
We can set certain colors or patterns to act as functions.
Let's say we want to send a message, we can flash white while it's sending, and turn the light to green when it is sent, or red when it's not sent.
1. Connect the led strip to your Arduino
2. Create functions for flashing or simply turning your strip into a color (this is not necessary, but it helps to speed up the process. Otherwise you would have to type the same code a million times.)
3. Put these functions into the existing functions for sending, recieving etc.
## Troubleshoot
### Wi-Fi issues:
- Make sure your board is compatible with Wi-Fi.
- Make sure the Wi-Fi- credentials are correct.
- You can test connectivity using a temporary hotspot and checking if the arduino has connected to it.
### Feed Issues:
- Make sure you are connnected to a network.
- Make sure the credentails are correct.
- Make sure AdaFruit IO is seeeing changes in the feed. (This can be checked with the activity graph)
