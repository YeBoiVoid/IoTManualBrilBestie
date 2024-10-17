# Wecome to the manual for the Bril Besties
Bril Besties is a product that battles the lone poopers. With bril besties you are able to talk to your bestie while doing your business.
It uses Arduino's connected to Wi-Fi to form a connection, it makes use of a Telegram bot so can talk with your bestie without a phone.

**NOTE**: In the end I was unable to get the code working due to memory limitations. I tried fixing this by optimizing some of the code but I was unsuccesfull to make it work.
## What do you need
#### HardWare (Per Person)
- 1x Arduino
#### Software
- Arduino IDE
- AdaFruit IO Arduino
- Telegram (you can use the web version)
## Actually conencting everything together
Now is the time we connect everything together. 
 ### Step 1. Connect modules to the board
 The arduino's cant sent texts or make calls, this makes connecting the modules qutie easy as there are no modules to connect to. We only have to connect two things to the arduino, these are a bot on Telegram and the AdaFruit IO website. We'll go over this in the coming steps. For these steps we dont need to do anything in the IDE yet.
 
### Step 2. Set up Telegram to the arduino
1. Make a telegram account.
2. You can search BotFather, make sure to use the one **WITH** the checkmark.

![image](https://github.com/user-attachments/assets/a9fa2e64-e224-4712-9bf1-dbe8c3a49779)

4. Create a new bot and name it, type "/start" and follow the simple steps the BotFather provides.
5. BotFather provies you with a bot token, we will need this later

### Step 3. Set up AdaFruit IO
1. Create an AdaFruit account.
3. Create a new feed, do this by going to the "feeds" tab on the website after logging in.

![image](https://github.com/user-attachments/assets/231757ce-a14f-4ee8-bb11-a33895e6299e)
![image](https://github.com/user-attachments/assets/872ea3e4-3780-44e6-b609-47569a6bb746)
![image](https://github.com/user-attachments/assets/ef20c74d-50df-4760-aae7-897fad035bee)

6. I crossed my key out (obviously), but remember this key.

## Sender Side
### Step 3. Connect Telegram with your arduino
I used an example for the base, in this base you have to edit a few variables. Make sure you have the Library **"UniversalTelegramBot"** installed.

![image](https://github.com/user-attachments/assets/0f566916-64c3-48e8-9f03-9ab2f1821875)

I found this example in File > example > UniversalTelegramBot > esp8266 > EchoBot

![image](https://github.com/user-attachments/assets/efa711cc-b685-4664-adca-6f786b4fe4a2)

1. Change the BotToken variable to the token that BotFather gave you.
2. Change the Wi-Fi variables tom your hotspot credentials.
3. Change "**handleNewMessage()**" and send the message to the functions we are going to create in the next step.

### Step 4. Connect AdaFruit IO with your arduino
I used an example for the base, in this base you only have to edit a few variables to get the connection to work. Make sure you have the AdaFruit IO Arduino library installed. 

![image](https://github.com/user-attachments/assets/312e62e5-8b08-4fd1-9caa-d69f28b0af23)

I found the example at File > Example > AdaFruit IO Arduino > adafruitioarduino_06_digital_in.

![image](https://github.com/user-attachments/assets/49a2d86c-5fcc-4d9a-9d29-d7fdf06cf58e)

Since voice messages will not be doable, text will have to do. The AdaFruit IO cannot send words. This is quite troubling if you want to send messages. However, it can send numeric values. And the alphabeth exists, so all we need to do is create an function that converts words into numeric values. So a word like "hello" would be "8 5 9 9 15", these values are something we can send to the AdaFruit IO.
I did this by creating two functions:
Get numerical values from a single charachter.

```
int charToNumber(char c) {
  c = tolower(c);

  // Check if the character is between 'a' and 'z'
  if (c >= 'a' && c <= 'z') {
    return c - 'a' + 1;
  } else {
    return 30;  // Return 30 for spaces 
  }
}
```

Loop over the sentence and use function 1 to get an arrays of numbers.
After this we can loop through the array and pass every index to AdaFruit.

```
 void stringToNumArray(std::string input) {
  std::cout << "input string is " + input;  // print input for debugging
  int inputLength = input.length();
  int intArray[inputLength];
  for (int i = 0; i < inputLength; i++) {
    intArray[i] = charToNumber(input[i]);
    if (intArray[i] == -1) {
      intArray[i] = 30;
    }
    std::cout << "\n";
    std::cout << intArray[i];  // print for debugging
  }
}
```
The output will be:

![image](https://github.com/user-attachments/assets/675c0361-8122-48fd-baca-5a3e241aada6)

## Reciever side
### Recieve information from AdaFruit
On the reciever side the code is quite similar; we still connect to AdaFruit and the telegram bot. The only difference here is that we want to turn an array of ints back into a string to do this i got this code:
```
char numberToChar(int num) {
    if (num >= 1 && num <= 26) {
        return 'a' + (num - 1);  // Map 1-26 to 'a'-'z'
    } else if (num == 30) {
        return ' ';  // Map 30 to a space
    } else {
        return '?';  // Use '?' for any other invalid numbers
    }
}
std::string numArrayToString(int numArray[], size_t size) {
    std::string result = "";
    
    for (size_t i = 0; i < size; ++i) {
        result += numberToChar(numArray[i]);  
    }
    return result;
}

```
If we input something like ```int numArray[] = {8, 5, 12, 12, 15, 30, 23, 15, 18, 12, 4};``` we get "Hello World" out of it.

### Send the message to the telegram bot as a reply
We can send this message to the bot to send it as a reply, this is done through some simple code:
```
void handleNewMessages(int numNewMessages, String message)
{
  for (int i = 0; i < numNewMessages; i++)
  {
    bot.sendMessage(bot.messages[i].chat_id, message, "");
  }
}
```
We can input the message we recieved into this function to print out a message for us.
In this picture the output is the input but the same principle applies.

![image](https://github.com/user-attachments/assets/033fadc9-85f7-467a-95fb-6726e6a5e5d2)

After we get this message we can simply reply and the arduino's will take care of the rest.

## Troubleshoot
### Wi-Fi issues:
- Make sure your board is compatible with Wi-Fi.
- Make sure the Wi-Fi- credentials are correct.
- You can test connectivity using a temporary hotspot and checking if the arduino has connected to it.
### Feed Issues:
- Make sure you are connnected to a network.
- Make sure the credentails are correct.
- Make sure AdaFruit IO is seeeing changes in the feed. (This can be checked with the activity graph)
### Memory Issues:
Due to the arduino I was using we have very limited memory for use. Because of this I was unable to get this code to work. I tried a few things to optimize the code:
- In the code I was frequently referring to std::string for my string needs. This is not very memory efficient as I found out, so I changed the std::string to simple char[].
- I had quite a few Serial.println() in my code for debugging. I commented all of them out, this also did not fix the issue.
- I had quite some loops with and without Serial.println() for debugging and general use. These are by default also not the best for your memory, so i removed as many of them as I could.
After asking ChatGPT for more help he gave me a few things to try to resolve my problem:
- Allocate memory.
- limit the loops and the arrays for less memory use.
- Use the FreeMemory library to check the free memory I have, to help with debugging. This also didn't help sadly.
