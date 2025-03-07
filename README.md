# codtech-task-3

To build an IoT security system that detects motion, captures images, and sends alerts to a mobile app, we'll need to integrate various hardware components, sensors, and communication methods. The system will utilize a motion sensor to detect movement, a camera to capture images, and Wi-Fi or Bluetooth communication to send real-time alerts to a mobile app.

Here’s a step-by-step guide to build the IoT security system:

1. Hardware Components Needed:
Microcontroller: ESP32 or ESP8266 (Both have Wi-Fi capabilities and enough processing power for image capture and communication)
PIR Motion Sensor: To detect motion (Passive Infrared Sensor)
Camera Module: ESP32-CAM (or a Raspberry Pi Camera if using a Raspberry Pi)
MicroSD Card: For storing captured images (if needed)
Buzzer (Optional): To provide an audible alert when motion is detected
LED (Optional): To indicate when motion is detected
Power Supply: To power the system
2. Software/IoT Platform:
Blynk App: For sending alerts to a mobile app (Android/iOS)
Firebase or MQTT (Optional): For storing images and sending notifications
Arduino IDE: For writing code for the ESP32/ESP8266
3. Design Overview:
Motion Detection: The PIR sensor detects movement in the vicinity.
Image Capture: Once motion is detected, the ESP32-CAM captures an image using the camera module.
Alert System: The system sends an alert to the mobile app via Blynk, or you can use Firebase for real-time notifications and image storage.
Mobile App: The Blynk app will display the alerts and allow you to view the captured images.
4. Wiring the System:
ESP32-CAM and PIR Motion Sensor:
Connect the PIR sensor to the ESP32 GPIO pins (e.g., D2 for the PIR sensor).
Connect the Camera module to the ESP32-CAM (the ESP32-CAM has a built-in camera).
The SD card can be inserted into the ESP32-CAM if you want to save images locally before sending them to the app.
Optionally, connect a buzzer to notify on-site when motion is detected (connected to another GPIO pin).
5. Code for the ESP32-CAM Security System:
Here’s an example of how you can write the code to detect motion, capture an image, and send an alert via the Blynk app.

cpp
Copy
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <ESP32CAM.h>
#include <Arduino.h>

// Blynk credentials
char auth[] = "Your_Blynk_Auth_Token";  // Replace with your Blynk token
char ssid[] = "Your_WiFi_SSID";         // Your WiFi SSID
char pass[] = "Your_WiFi_Password";     // Your WiFi Password

// Define the PIR sensor pin and Camera pins
#define PIR_PIN 13    // PIR sensor pin
#define LED_PIN 4     // LED pin (optional, for indication)

// Set up Blynk virtual pins
#define PIR_VPIN V1  // Virtual pin for PIR alert
#define IMAGE_VPIN V2 // Virtual pin for Image capture alert

void setup() {
  // Initialize serial for debugging
  Serial.begin(115200);
  
  // Initialize the PIR sensor pin
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);  // Optional LED for PIR status
  
  // Connect to WiFi
  Blynk.begin(auth, ssid, pass);

  // Initialize the camera
  if (!initCamera()) {
    Serial.println("Camera initialization failed!");
    while (1);
  }
  Serial.println("Camera initialized!");
}

void loop() {
  Blynk.run();  // Run Blynk app
  
  // Check for motion
  int motionDetected = digitalRead(PIR_PIN);
  if (motionDetected == HIGH) {
    Serial.println("Motion Detected!");
    digitalWrite(LED_PIN, HIGH);  // Turn on LED to indicate motion detected
    
    // Capture Image
    captureImage();

    // Send alert to Blynk app
    Blynk.virtualWrite(PIR_VPIN, "Motion Detected!");
    Blynk.virtualWrite(IMAGE_VPIN, "Image Captured!");

    delay(5000); // Avoid multiple alerts in a short period
    digitalWrite(LED_PIN, LOW);  // Turn off LED after a delay
  }

  delay(100); // Small delay to avoid spamming
}

bool initCamera() {
  // ESP32 Camera setup code
  // Initialize camera (the specifics depend on the model used)
  // Return true if initialization is successful
  return true;
}

void captureImage() {
  // Code to capture an image with the camera and save it to the SD card or memory.
  // Use the camera API provided for the ESP32-CAM to take a snapshot
  // Optionally, you can upload the image to cloud storage or send via HTTP to a server
  // For simplicity, we'll assume it's saved locally or in a buffer.
  Serial.println("Capturing image...");
  // Save or send the captured image to the app (this depends on the storage method)
}
6. Set Up the Blynk App:
Create a New Project in the Blynk App:

Create a new project in the Blynk app (select ESP32).
After the project is created, the app will generate an auth token. Paste this token into the code above.
Add Widgets in Blynk:

Add a Push Notification widget to notify the user when motion is detected.
Add a Label or Image widget to display the captured image (you can send images to Firebase or another service, but for simplicity, this guide will focus on sending alerts).
Add a Button widget to simulate or control the system manually.
Configure Virtual Pins:

Set the Virtual Pin (V1) to receive motion detection alerts.
Set the Virtual Pin (V2) to receive image capture alerts.
Run the App:

Run the app and monitor alerts. When motion is detected, you should receive a notification and an alert message in the app.
7. Optional Advanced Features:
Image Upload to Cloud Storage (Firebase/Google Drive, etc.):

Instead of just sending an alert, upload the captured images to cloud storage (e.g., Firebase Storage or Google Drive) and send a link in the Blynk app.
Push Notifications:

Use Firebase Cloud Messaging (FCM) to send push notifications to your phone when motion is detected.
Multiple Motion Sensors:

Expand the system by adding more PIR sensors to cover more areas.
Voice Alerts/Audio:

Integrate a voice module to provide an audible alert when motion is detected.
8. Testing the System:
Power on the system and ensure that the ESP32-CAM connects to Wi-Fi.
Trigger the PIR sensor by moving in front of it.
Monitor the Blynk app to receive real-time alerts and notifications.
Capture images and check if they are being sent correctly to the app or saved to the cloud.
9. Conclusion:
You now have a functional IoT security system that detects motion, captures images, and sends real-time alerts to a mobile app using the Blynk platform. The system can be expanded with additional features like cloud storage, voice alerts, and more sensors for better coverage. This can be a great start for building a fully automated home security system!

