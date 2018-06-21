test2---
layout: post
title: "첫 블로그 시작입니다."
date: 2018-06-21 19:48:00 +0900
comments: false
---

### 블로그 시작

겁나 어려워요.

[arduino ide download](https://www.arduino.cc/en/Main/Software) 여기에서 다운로드를 받아서 인스톨을 먼저해야된다.

그다음에 아두이노 ide가 실행 되면 아래의 코드를 붙혀 넣고 업로드를 진행하면 된다. ^^

```

#include <Servo.h>

Servo myservo;
int dcmPin1 = 3;
int dcmPin2 = 11;
int servoPin = 13;

int POW_SW_Pin = 8;
int PAN_SW_Pin = 7;
int LV1_SW_Pin = 6;
int LV2_SW_Pin = 5;
int LV3_SW_Pin = 4;

int pos = 90;
int Dir_Servo = 0;
unsigned char dcmPower = 50;
int PAN_OnOff = 0;
int POW_OnOff = 0;
int POW_LEVEL = 1;

int Key_Data[10] = {HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH};
int Key_Data_Before[10] = {HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH};

void setup() {

  myservo.attach(servoPin);
  myservo.write(90);

  pinMode(POW_SW_Pin, INPUT);
  pinMode(PAN_SW_Pin, INPUT);
  pinMode(LV1_SW_Pin, INPUT);
  pinMode(LV2_SW_Pin, INPUT);
  pinMode(LV3_SW_Pin, INPUT);

  pinMode(dcmPin1, OUTPUT);
  pinMode(dcmPin2, OUTPUT);
  digitalWrite(dcmPin1, LOW);
  digitalWrite(dcmPin2, LOW);
  analogWrite(dcmPin1, 0);
  analogWrite(dcmPin2, 0);
  delay(1000);
}

void loop() {
    readKey();
  if (POW_OnOff == HIGH)
  {
    if (POW_LEVEL == 1 ) analogWrite(dcmPin2, 85);
    else if (POW_LEVEL == 2 ) analogWrite(dcmPin2, 175);
    else if (POW_LEVEL == 3 ) analogWrite(dcmPin2, 255);
    else;
    analogWrite(dcmPin1, 0);
    PAN_Control();
    delay(60);
  }
  else if (POW_OnOff == LOW)
  {
    analogWrite(dcmPin1, 0);
    analogWrite(dcmPin2, 0);
    delay(100);
  }
  else;
}

void PAN_Control()
{
  if (PAN_OnOff == HIGH)
  {
    if (Dir_Servo == LOW)
    {
      if (pos < 150)
      {
        pos++;
        myservo.write(pos);
      }
      else Dir_Servo = HIGH;
    }
    else
    {
      if (pos > 30)
      {
        pos--;
        myservo.write(pos);
      }
      else Dir_Servo = LOW;
    }
  }
  else;
}

void readKey()
{
  for (int i = POW_SW_Pin; i >= LV3_SW_Pin; i--)
  {
    Key_Data[i] = digitalRead(i);
    if (Key_Data[i] == LOW && Key_Data[i] != Key_Data_Before[i])
    {
      Key_Data_Before[i] = Key_Data[i];
      if (i == POW_SW_Pin) {
        if (POW_OnOff == LOW) POW_OnOff = HIGH;
        else POW_OnOff = LOW;
      }
      else if (i == PAN_SW_Pin) {
        if (PAN_OnOff == LOW) PAN_OnOff = HIGH;
        else PAN_OnOff = LOW;
      }
      else if (i == LV1_SW_Pin) POW_LEVEL = 1;
      else if (i == LV2_SW_Pin) POW_LEVEL = 2;
      else if (i == LV3_SW_Pin) POW_LEVEL = 3;
      else;
      delay(200);
    }
    else if (Key_Data[i] == HIGH && Key_Data[i] != Key_Data_Before[i])
    {
      Key_Data_Before[i] = Key_Data[i];
    }
    else;
  }
}



```

# 참조
-----
* [arduino](https://www.arduino.cc/)
* [아두이노 호환보드](http://smartstore.naver.com/domekit/products/308227621)
* [아두니노 선풍기 프레임](http://smartstore.naver.com/domekit/products/2574181501)
* [ch340 driver](https://sparks.gogo.co.nz/ch340.html)
