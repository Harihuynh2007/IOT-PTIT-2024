
# IoT Face Mask Detection System with ESP32-CAM and Teachable Machine

This guide walks you through building an IoT system using the ESP32-CAM to detect if a person is wearing a face mask. The project utilizes the Teachable Machine for machine learning and Telegram for notifications.

## Prerequisites
- **Hardware:**
  - ESP32-CAM
  - Micro-USB cable
  - Laptop or PC

- **Software:**
  - [Arduino IDE](https://www.arduino.cc/en/software)
  - Thư viện ESP32 (ESP32-CAM support)

## Step 1: Set Up Hardware and Install Software
1. **Connect the ESP32-CAM**:
   - Use a micro-USB cable to connect the ESP32-CAM to your laptop.

2. **Install Arduino IDE**:
   - Download and install the [Arduino IDE](https://www.arduino.cc/en/software).

3. **Install ESP32 Library in Arduino IDE**:
   - Open Arduino IDE and go to `File > Preferences`.
   - Add the following URL to the "Additional Boards Manager URLs" field:
     ```
     https://dl.espressif.com/dl/package_esp32_index.json
     ```
   - Go to `Tools > Board > Boards Manager`, search for "ESP32," and install it.

## Step 2: Configure and Connect the ESP32-CAM
1. **Select Board and Port**:
   - In Arduino IDE, go to `Tools > Board` and select "AI Thinker ESP32-CAM."
   - Go to `Tools > Port` and choose the appropriate COM port for the ESP32-CAM.

## Step 3: Create a Model with Teachable Machine
1. Visit [Teachable Machine](https://teachablemachine.withgoogle.com/) and create an "Image Project."
2. **Create Two Classes**:
   - **Class 1**: "Wearing a mask" – Upload images of people wearing masks.
   - **Class 2**: "Not wearing a mask" – Upload images of people not wearing masks.
3. **Train the Model** and export it:
   - Click "Train Model" to let the system learn from the images.
   - Click "Export Model," select "TensorFlow Lite," and choose "Float16" format to download the `.tflite` file.

## Step 4: Upload Code to ESP32-CAM via Arduino IDE
1. **Install Required Libraries**:
   - Make sure to install the following libraries:
     - **ESP32**
     - **Camera** (integrated with the ESP32 library)
     - **HTTPClient** (to call APIs)

2. **Write the Code**:
   - Use the sample code below to capture images and send them to the Teachable Machine API:
   ```cpp
   #include "esp_camera.h"
   #include <WiFi.h>
   #include <HTTPClient.h>

   // WiFi settings
   const char* ssid = "Your_SSID";
   const char* password = "Your_PASSWORD";

   // API URL
   const char* api_url = "https://teachablemachine.withgoogle.com/models/YOUR_MODEL/model.json";

   void setup() {
       Serial.begin(115200);
       WiFi.begin(ssid, password);
       while (WiFi.status() != WL_CONNECTED) {
           delay(1000);
           Serial.println("Connecting to WiFi...");
       }
       Serial.println("Connected to WiFi");

       // Camera setup here (refer to ESP32-CAM documentation)
   }

   void loop() {
       // Capture image, send it to the API, and process the response
       delay(5000);
   }
   ```

   - Replace Your_SSID, Your_PASSWORD, and YOUR_MODEL with your WiFi credentials and model URL.
3. **Upload the Code**:
  - Select the correct COM port under Tools > Port.
  - Click "Upload" to flash the code onto the ESP32-CAM. This process may take a few minutes.
    
## Step 5: Integrate with Telegram for Notifications
1.**Create a Telegram Bot**:
  - Open Telegram, search for "BotFather," and create a new bot.
  - Copy the token API of the bot for use in your Arduino code
    .
2.**Update Code for Telegram Communication**:
  - Use the HTTPClient library in the Arduino code to send requests and receive messages via Telegram.
    
## Step 6: Test the System
1.**Run the System**:
  - Place a person wearing or not wearing a mask in front of the ESP32-CAM.
  - Observe the response sent to the Telegram bot to verify the system's accuracy.
    
2.**Adjust the Model**:
  - If the system does not correctly identify masks, retrain the model on Teachable Machine and update the code as necessary.
    
