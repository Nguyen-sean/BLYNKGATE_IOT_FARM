
# IoT Project: Temperature and Humidity Monitoring with Arduino Uno and BlynkGate

This project demonstrates how to read temperature and humidity values using the **DHT11** or **DHT22** sensor and send the data to the **Blynk** cloud using the **BlynkGate** module. The project uses an **Arduino Uno** and allows remote monitoring of environmental conditions.

## Table of Contents
- [Overview](#overview)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Circuit Diagram](#circuit-diagram)
- [Installation](#installation)
- [Usage](#usage)
- [License](#license)

## Overview

In this project, we will:
- Use a **DHT11** or **DHT22** sensor to measure temperature and humidity.
- Connect the **Arduino Uno** to the **Blynk** cloud using the **BlynkGate** Wi-Fi module.
- Send sensor data to the **Blynk** cloud and visualize it using the Blynk app.

## Hardware Requirements

- **Arduino Uno**
- **BlynkGate Wi-Fi Module**
- **DHT11** or **DHT22** sensor
- Jumper wires
- Breadboard

## Software Requirements

- **Arduino IDE**: [Download](https://www.arduino.cc/en/software)
- **Blynk Library**: Install it from the Arduino Library Manager (`BlynkSimpleEsp8266.h`)
- **DHT Library**: Install it from the Arduino Library Manager (`DHT.h`)
- **BlynkGate Library**: Provided by Makerlabvn, install it manually from their [GitHub repository](https://github.com/makerlabvn/makerlabvnlib)

## Circuit Diagram

| **Pin**      | **DHT11/22**  |
|--------------|---------------|
| VCC          | 5V            |
| GND          | GND           |
| Data (Pin 2) | Digital Pin 4 |

| **Pin**      | **BlynkGate**  |
|--------------|----------------|
| VCC          | 3.3V/5V        |
| GND          | GND            |
| RX           | TX (Pin 1)     |
| TX           | RX (Pin 0)     |

## Installation

1. **Connect the Arduino Uno to the DHT11/22 sensor** and **BlynkGate module** according to the circuit diagram.
2. **Install the necessary libraries**:
   - Open Arduino IDE.
   - Go to **Tools > Manage Libraries** and search for `Blynk` and `DHT`.
   - Install both libraries.
3. **Upload the code to Arduino Uno**:
   - Open the `Arduino IDE`.
   - Copy and paste the following code into the Arduino IDE:

```cpp
/*
  Title:  Temperature and Humidity demo
  Description: Read value of DHT11/22 sensor and send to Blynk every 5s.
              - Virtual Pin V1 (for temperature)
              - Virtual Pin V2 (for humidity)
*/

// Blynk setup
#define BLYNK_TEMPLATE_ID "Copy_BLYNK_TEMPLATE_ID_From_BlynkCloud"
#define BLYNK_TEMPLATE_NAME "Copy_BLYNK_TEMPLATE_NAME_From_BlynkCloud"
#define BLYNK_AUTH_TOKEN "Copy_BLYNK_AUTH_TOKEN_From_BlynkCloud"

#include "BlynkGate.h"
#include "DHT.h"

// Pin setup for DHT
#define DHTPIN D4 // Pin for data from DHT sensor
#define DHTTYPE DHT11   // or DHT22 if you're using DHT22
DHT dht(DHTPIN, DHTTYPE);

char ssid[] = "Your_SSID";
char pass[] = "Your_WIFI_Password";

BlynkTimer timer;

void sendSensorData() {
  float temp = dht.readTemperature();
  float humidity = dht.readHumidity();
  
  // Send to Blynk
  Blynk.virtualWrite(V1, temp); // send temperature to Blynk
  Blynk.virtualWrite(V2, humidity); // send humidity to Blynk

  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.print(" *C, Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");
}

void setup() {
  Serial.begin(115200);
  dht.begin();
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Setup timer to send sensor data every 5 seconds
  timer.setInterval(5000L, sendSensorData);
}

void loop() {
  Blynk.run();
  timer.run();
}
```

4. **Get your Blynk Cloud credentials**:
   - Go to [Blynk Cloud](https://blynk.cloud/) and create a new template.
   - Copy the `BLYNK_TEMPLATE_ID`, `BLYNK_TEMPLATE_NAME`, and `BLYNK_AUTH_TOKEN`.
   - Replace the placeholders in the code with your credentials.
   
5. **Upload the code to Arduino** and make sure it connects to Wi-Fi.

## Usage

1. **Run the Blynk app**:
   - Create a new project in the Blynk app.
   - Add two widgets (Gauge or Value Display) for temperature and humidity.
   - Assign **Virtual Pin V1** to temperature and **Virtual Pin V2** to humidity.

2. **View the data**:
   - Once the code is running and the device is connected to Wi-Fi, you can monitor the temperature and humidity values in real-time through the Blynk app.

## License

This project is open-source and available under the MIT License. Feel free to modify and use it for your IoT projects.
