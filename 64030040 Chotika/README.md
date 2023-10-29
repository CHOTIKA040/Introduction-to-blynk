# Introduction-to-blynk

## Slides

https://github.com/Special-Topics-Computer-2023/Introduction-to-blynk/blob/main/Docs/blynk.pdf

## Code 

https://gist.github.com/koson/252e2439c6a8db0fa0d315e3269c88b7
 

## Assignment

ปรับปรุงหน้า dashboard  ให้แสดง สถานะของ LED, ค่าแรงดันไฟฟ้า (จาก volume), ค่าแสง จาก LDR, ค่าอุณหภูมิ + ค่าความชื้น (DHT11)

ตัวอย่าง

![image](https://github.com/Special-Topics-Computer-2023/Introduction-to-blynk/assets/567256/b46d9d57-93ff-4198-b80e-f5b7a01c3e7b)


## การส่งงาน

Capture หน้า dashboard เป็นคลิปสั้นๆ แสดงให้เห็นถึงการทำงานของระบบ 

upload ขึ้น youtube เป็น video ที่ดูได้สำหรับคนที่มี link (หรือถ้าแน่ก็เปิดเป็น public ไปเลย)

ส่ง link มาเป็น pull request

```
/*************************************************************

  This is a simple demo of sending and receiving some data.
  Be sure to check out other examples!
 *************************************************************/

/* Fill-in information from Blynk Device Info here */
#define BLYNK_TEMPLATE_ID           "TMPL6aCohDuPw"
#define BLYNK_TEMPLATE_NAME         "Quickstart Template"
#define BLYNK_AUTH_TOKEN            "eERd03gDye_3Zysgxgu8afZGrMkYp_Ev"

/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial


#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include "DHT.h"
// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "Pattana_WIFI-2.4G";
char pass[] = "0947879319";

#define LED_PIN 2
#define DHTTYPE DHT11
#define DHTPIN 17

const int potPin = 34;
int potValue = 0;
const int luxPin = 39;
int luxValue = 0;
DHT dht(DHTPIN, DHTTYPE);
BlynkTimer timer;

BLYNK_WRITE(V0)
{
  int value = param.asInt();
  if (value == 1) {
    digitalWrite(LED_PIN, HIGH);
    Serial.print("value =");
    Serial.println(value);
  } else {
    digitalWrite(LED_PIN, LOW);
    Serial.print("value = ");
    Serial.println(value);
  }
}

void readPot()
{

  potValue = analogRead(potPin);
  Serial.println ("Pot Value = " + String(potValue / 40.95));
  Blynk.virtualWrite(V7, potValue);
  luxValue = analogRead(luxPin);
  Serial.println ("Lux Value = " + String(luxValue / 40.95));
  Blynk.virtualWrite(V6, luxValue);
  int h = dht.readHumidity();
  Serial.println ("Humidity Value = " + String(h));
  Blynk.virtualWrite(V5, h);
  float t = dht.readTemperature();
  Serial.println ("Temperature Value = " + String(t));
  Blynk.virtualWrite(V4, t);


}

BLYNK_CONNECTED()
{
  Blynk.setProperty(V3, "offImageUrl", "https://static-image.nyc3.cdn.digitaloceanspaces.com/general/fte/congratulations.png");
  Blynk.setProperty(V3, "onImageUrl",  "https://static-image.nyc3.cdn.digitaloceanspaces.com/general/fte/congratulations_pressed.png");
  Blynk.setProperty(V3, "url", "https://docs.blynk.io/en/getting-started/what-do-i-need-to-blynk/how-quickstart-device-was-made");
}

void myTimerEvent()
{
  Blynk.virtualWrite(V2, millis() / 1000);
}
void setup()
{
  dht.begin();
  Serial.begin(115200);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(1000L, myTimerEvent);
  timer.setInterval(2000L, readPot);
  Serial.println("Init Timer ");
  pinMode(LED_PIN, OUTPUT);
}

void loop()
{
  Blynk.run();
  timer.run();
  delay(10);
}
```

## Link Video
* https://www.youtube.com/watch?v=AadBz_z6ClA
