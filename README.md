output
code
#define BLYNK_TEMPLATE_ID "TMPL3wpFP3MEc"
#define BLYNK_TEMPLATE_NAME "medicine reminder"
#define BLYNK_AUTH_TOKEN "zBneVJLkNr4pI6J3mKTwEQEGImWETtuP"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

const int soundSensorPin = A0;
const int motionSensorPin = 2;

char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Redmi Note 10S";
char pass[] = "12345678";

BlynkTimer timer;
int soundFlag = 0;
int motionFlag = 0;

void sendSoundSensor() {
  int soundValue = analogRead(soundSensorPin);
  Serial.print("Sound sensor value: ");
  Serial.println(soundValue);

  int soundThreshold = 500;  // Adjust as needed

  if (soundValue > soundThreshold && soundFlag == 0) {
    Serial.println("Sound detected!");
    
    soundFlag = 1;
  } else if (soundValue <= soundThreshold) {
    Serial.println("No sound detected");
    soundFlag = 0;
  }
}

void sendMotionSensor() {
  int motionValue = digitalRead(motionSensorPin);

  if (motionValue == HIGH && motionFlag == 0) {
    Serial.println("Motion detected!");
    Blynk.logEvent("motion_alert", "Motion Detected");
    motionFlag = 1;
    Blynk.logEvent("medicine_reminder");
  } else if (motionValue == LOW) {
    Serial.println("No motion detected");
    motionFlag = 0;
  }
}

void setup() {
  pinMode(soundSensorPin, INPUT);
  pinMode(motionSensorPin, INPUT);

  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);

  timer.setInterval(5000L, sendSoundSensor);  // Adjust the interval as needed
  timer.setInterval(5000L, sendMotionSensor); // Adjust the interval as needed
}

void loop() {
  Blynk.run();
  timer.run();
}
![WhatsApp Image 2023-11-24 at 09 00 49_7776724a](https://github.com/Anirudh0243/output/assets/149662283/bcea882a-4db7-4c1d-938e-35f7447a88ee)
![WhatsApp Image 2023-11-23 at 21 40 19_f3e1ee4f](https://github.com/Anirudh0243/output/assets/149662283/0b2b221f-50cd-450e-a094-a2c4e51513c7)
![WhatsApp Image 2023-11-23 at 21 44 21_886410b3](https://github.com/Anirudh0243/output/assets/149662283/b58d9832-dbef-4fa6-bc65-e318b4e820a5)
![WhatsApp Image 2023-11-23 at 22 21 27_e9e31b9f](https://github.com/Anirudh0243/output/assets/149662283/ceaa5fdf-9014-4998-bdbb-1017d4e1221f)


