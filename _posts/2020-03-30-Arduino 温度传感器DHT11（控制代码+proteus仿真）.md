---
layout: post
title: Arduino 温度传感器DHT11(控制代码+proteus仿真)
categories: Arduino
description: 温度传感器DHT11的arduino控制代码和proteus仿真，学习使用
keywords: Arduino, 温度传感器, DHT11
---

仿真图如下：

![2020-03-30-Arduino-温度传感器DHT11-控制代码-proteus仿真](/images/posts/2020-03-30-Arduino-温度传感器DHT11-控制代码-proteus仿真/2020-03-30-Arduino-温度传感器DHT11-控制代码-proteus仿真.png)

Arduino控制代码如下：

main.ino

```
#include <dht11.h>
dht11 DHT11;
#define DHT11PIN 2
void setup(){
  Serial.begin(9600);
}
​
void loop(){
  Serial.println("\n");
  int chk = DHT11.read(DHT11PIN);
   Serial.print("Read sensor: ");
  switch (chk)
  {
    case DHTLIB_OK:
                Serial.println("OK");
                break;
    case DHTLIB_ERROR_CHECKSUM:
                Serial.println("Checksum error");
                break;
    case DHTLIB_ERROR_TIMEOUT:
                Serial.println("Time out error");
                break;
    default:
                Serial.println("Unknown error");
                break;
  }
​
  Serial.print("Humidity (%): ");
  Serial.println((float)DHT11.humidity, 2);
​
  Serial.print("Temperature (oC): ");
  Serial.println((float)DHT11.temperature, 2);
delay(2000);
}
```
注：附件压缩包有所有文件



相关库文件代码如下：

dht11.h

```
#ifndef dht11_h
#define dht11_h
​
#if defined(ARDUINO) && (ARDUINO >= 100)
#include <Arduino.h>
#else
#include <WProgram.h>
#endif
​
#define DHT11LIB_VERSION "0.4.1"
​
#define DHTLIB_OK        0
#define DHTLIB_ERROR_CHECKSUM  -1
#define DHTLIB_ERROR_TIMEOUT  -2
​
class dht11
{
public:
    int read(int pin);
  int humidity;
  int temperature;
};
#endif
//
// END OF FILE
//
```
dht11.cpp

```
//
//    FILE: dht11.cpp
// VERSION: 0.4.1
// PURPOSE: DHT11 Temperature & Humidity Sensor library for Arduino
// LICENSE: GPL v3 (http://www.gnu.org/licenses/gpl.html)
//
// DATASHEET: http://www.micro4you.com/files/sensor/DHT11.pdf
//
// HISTORY:
// George Hadjikyriacou - Original version (??)
// Mod by SimKard - Version 0.2 (24/11/2010)
// Mod by Rob Tillaart - Version 0.3 (28/03/2011)
// + added comments
// + removed all non DHT11 specific code
// + added references
// Mod by Rob Tillaart - Version 0.4 (17/03/2012)
// + added 1.0 support
// Mod by Rob Tillaart - Version 0.4.1 (19/05/2012)
// + added error codes
//
​
#include "dht11.h"
​
// Return values:
// DHTLIB_OK
// DHTLIB_ERROR_CHECKSUM
// DHTLIB_ERROR_TIMEOUT
int dht11::read(int pin)
{
  // BUFFER TO RECEIVE
  uint8_t bits[5];
  uint8_t cnt = 7;
  uint8_t idx = 0;
​
  // EMPTY BUFFER
  for (int i=0; i< 5; i++) bits[i] = 0;
​
  // REQUEST SAMPLE
  pinMode(pin, OUTPUT);
  digitalWrite(pin, LOW);
  delay(18);
  digitalWrite(pin, HIGH);
  delayMicroseconds(40);
  pinMode(pin, INPUT);
​
  // ACKNOWLEDGE or TIMEOUT
  unsigned int loopCnt = 10000;
  while(digitalRead(pin) == LOW)
    if (loopCnt-- == 0) return DHTLIB_ERROR_TIMEOUT;
​
  loopCnt = 10000;
  while(digitalRead(pin) == HIGH)
    if (loopCnt-- == 0) return DHTLIB_ERROR_TIMEOUT;
​
  // READ OUTPUT - 40 BITS => 5 BYTES or TIMEOUT
  for (int i=0; i<40; i++)
  {
    loopCnt = 10000;
    while(digitalRead(pin) == LOW)
      if (loopCnt-- == 0) return DHTLIB_ERROR_TIMEOUT;
​
    unsigned long t = micros();
​
    loopCnt = 10000;
    while(digitalRead(pin) == HIGH)
      if (loopCnt-- == 0) return DHTLIB_ERROR_TIMEOUT;
​
    if ((micros() - t) > 40) bits[idx] |= (1 << cnt);
    if (cnt == 0)   // next byte?
    {
      cnt = 7;    // restart at MSB
      idx++;      // next byte!
    }
    else cnt--;
  }
​
  // WRITE TO RIGHT VARS
        // as bits[1] and bits[3] are allways zero they are omitted in formulas.
  humidity    = bits[0]; 
  temperature = bits[2];
​
  uint8_t sum = bits[0] + bits[2];
​
  if (bits[4] != sum) return DHTLIB_ERROR_CHECKSUM;
  return DHTLIB_OK;
}
//
// END OF FILE
//
```


压缩包附件：[下载地址](/attachment/posts/2020-03-30-Arduino-温度传感器DHT11-控制代码-proteus仿真/Arduino328-DHT11.rar)

