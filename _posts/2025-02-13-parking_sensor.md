---
title: Parking Sensor
description: A compact breadboard project designed to explore the construction of a practical parking aid using an ultrasonic sensor, ESP32 microcontroller, buzzer, and LED for real-time proximity alerts.
date: 2025-02-13 00:00:00 -500
categories: [IT project]
tags: [electronics,embedded systems]
image: /assets/parking_sensor/logo.jpg
---

## Overview
This "Parking Sensor" is a compact breadboard project designed to explore the construction of a parking aid. It utilizes an ultrasonic sensor to measure distances and employs both a buzzer and an LED for real-time proximity alerts. The project is centered around an ESP32 microcontroller, which orchestrates these components.

## Motivation
Initiated as a post-course practice project following my first semester in critical systems, this endeavor aimed to apply and expand upon classroom learnings in electronics. The outcome, this parking sensor, serves as a fundamental illustration of using an ultrasonic sensor and actuators like a buzzer and LED to create a straightforward yet effective application.

## Layout
Displayed below is the component arrangement on the breadboard:

<img src="/assets/parking_sensor/layout.png" alt="layout" style="width:560px; border-radius:10px;">

## Demonstration

For a visual demonstration, watch the parking sensor in action in the following video:

<div style="display: flex; justify-content: center;">
  <iframe width="560" height="315" 
      src="https://www.youtube.com/embed/9UZhAGEfxdU" 
      title="YouTube video player" 
      frameborder="0" 
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
      allowfullscreen
      style="border-radius: 10px; overflow: hidden;">
  </iframe>
</div>

## Code

Here is the complete code written in Arduino IDE, resembling a C++ syntax:

```cpp
#include <Wire.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

#define echoPin 15               // Change this pin number if necessary
#define trigPin 4               // Change this pin number if necessary
#define buzzer 12
#define factor 100
#define ledPin 33

long distance;
volatile int frequency = 0; // Shared variable between tasks

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(ledPin, OUTPUT);

  // Create two tasks: one for measuring distance, one for controlling the buzzer
  xTaskCreatePinnedToCore(measureDistance, "Distance Task", 1024, NULL, 1, NULL, 0);
  xTaskCreatePinnedToCore(controlBuzzer, "Buzzer Task", 1024, NULL, 1, NULL, 1);
}

void loop() {
  // FreeRTOS handles tasks; no code needed here
}

void measureDistance(void *pvParameters) {
  unsigned long previousMillis = 0;

  for (;;) {
    unsigned long currentMillis = millis();

    // Ensure the ultrasonic sensor reads every 200 ms
    if (currentMillis - previousMillis >= 500) {
      previousMillis = currentMillis;

      distance = measureDistance();
        // Display the distance in the serial monitor
      Serial.print("Distance: ");
      Serial.print(distance);
      Serial.println(" cm");

      // Update the frequency based on distance

      if (distance > 0 && distance <= 10) {
        frequency = -1; // Constant beep for very close objects
      } else if (distance > 80) {
        frequency = 0; // No beeps if out of range
      } else {
        frequency = factor /distance; //Calculate frequency
      }
    }
    vTaskDelay(1 / portTICK_PERIOD_MS); // Yield to other tasks
  }
}

void controlBuzzer(void *pvParameters) {
  const int minDelay = 20; // Minimum delay in ms to ensure stability
  unsigned long lastBeepTime = 0; // Track the last beep time

  for (;;) {
    if (frequency == 0) { // No tone
      noTone(buzzer);
      vTaskDelay(100 / portTICK_PERIOD_MS);
    } else if (frequency == -1) { // Continuous tone
      digitalWrite(ledPin, HIGH);
      tone(buzzer, 1000);
      vTaskDelay(100 / portTICK_PERIOD_MS);
      digitalWrite(ledPin, LOW);
    } else {
      unsigned long currentMillis = millis();
      int beepDelay = max(1000 / frequency, minDelay); // Ensure a minimum delay
      if (currentMillis - lastBeepTime >= beepDelay) {
        lastBeepTime = currentMillis;
        digitalWrite(ledPin, HIGH);
        tone(buzzer, 1000, 100); // Beep for 100 ms
        digitalWrite(ledPin, LOW);
      }
      vTaskDelay(10 / portTICK_PERIOD_MS); // Small delay to allow CPU to handle other tasks
    }
  }
}



long measureDistance(){
  // Trigger the ultrasonic sensor
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure the duration of the echo
  long duration = pulseIn(echoPin, HIGH);

  // Calculate the distance in centimeters
  long distance = duration / 58.2;

  return distance;
}
  ```

## Conclusion

This project was a fun application of first-semester learnings in critical systems. It not only reinforced theoretical concepts but also introduced practical implementation using FreeRTOS for multitasking on the ESP32 microcontroller. Such projects, even if modest in complexity, are engaging and highlight the integration of physical actuators with software systems.
