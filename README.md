# CSN-150 Final Project Documentation
## Sending WhatsApp Messages with ESP32
This can be useful for receiving notifications from the ESP32, such as sensor readings, alerts when values exceed or fall below defined thresholds, motion detection notifications, and a wide range of other applications.

### Requirements
1. ESP32 development board
2. USB cable to program the ESP32
3. A WhatsApp account
4. CallMeBot WhatsApp API Key
5. URLEncode Library installed

### Installing Arduino IDE

* Visit: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)
* Download and install the IDE 

### Installing ESP32 Boards

1. Open the Arduino IDE and navigate to "File"
2. Click on "Preferences"
3. In the "Additional Boards Manager URLs" field, add the following URL: (https://github.com/espressif/arduino-esp32/blob/master/package/package_esp32_index.template.json)

### Install URLEncode Library
This library is used to format the message into a valid URL.
Go to Sketch > Include Library > Manage Libraries and search for URLEncode library by Masayuki Sugahara


![image](https://github.com/user-attachments/assets/d7bcd674-7125-47c6-8711-c21d52457e4e)




### Getting CallMeBot WhatsApp API Key
Setup:
You need to get the apikey form the bot before using the API:
1. Add the phone number **+34 694 242 562** into your Phone Contacts
2. Send this message *"I allow callmebot to send me messages"* to the new Contact created (using WhatsApp of course)
3. Wait until you receive the message "API Activated for your phone number. "`Your APIKEY is 123123`" from the bot.
   Note: If you don't receive the ApiKey in 2 minutes, please try again after 24hs.
4. The WhatsApp message from the bot will contain the apikey needed to send messages using the API.
   You can send text messages using the API after receiving the confirmation.


### Code to Send WhatsApp Messages from ESP32
Here is a complete example code that sends a WhatsApp message: [https://randomnerdtutorials.com/esp32-send-messages-whatsapp/)

```
  /* 
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/esp32-send-messages-whatsapp/
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files.
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
*/

#include <WiFi.h>    
#include <HTTPClient.h>
#include <UrlEncode.h>

const char* ssid = "REPLACE_WITH_YOUR_SSID";
const char* password = "REPLACE_WITH_YOUR_PASSWORD";

// +international_country_code + phone number
// Portugal +351, example: +351912345678
String phoneNumber = "REPLACE_WITH_YOUR_PHONE_NUMBER";
String apiKey = "REPLACE_WITH_API_KEY";

void sendMessage(String message){

  // Data to send with HTTP POST
  String url = "https://api.callmebot.com/whatsapp.php?phone=" + phoneNumber + "&apikey=" + apiKey + "&text=" + urlEncode(message);    
  HTTPClient http;
  http.begin(url);

  // Specify content-type header
  http.addHeader("Content-Type", "application/x-www-form-urlencoded");
  
  // Send HTTP POST request
  int httpResponseCode = http.POST(url);
  if (httpResponseCode == 200){
    Serial.print("Message sent successfully");
  }
  else{
    Serial.println("Error sending the message");
    Serial.print("HTTP response code: ");
    Serial.println(httpResponseCode);
  }

  // Free resources
  http.end();
}

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.println("Connecting");
  while(WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Connected to WiFi network with IP Address: ");
  Serial.println(WiFi.localIP());

  // Send Message to WhatsAPP
  sendMessage("Hello from ESP32!");
}

void loop() {
  
}
```


### In the code, there are a few details you need to customize. I had to enter the following:
My network SSID and password
My phone number in international format
The API key I received after consenting to receive messages via the CallMeBot API


### Output
Once uploaded, open the Serial Monitor. You should see:
```
Connecting to WiFi...
Connected!
Message sent successfully!
OK
```


And you’ll receive a WhatsApp message:
**"Hello from ESP32! "**


![image](https://github.com/user-attachments/assets/57af7104-8811-4a8c-a077-aee4b3106669)

### Conclusion
I successfully set up your ESP32 to send WhatsApp messages using the CallMeBot API. 





