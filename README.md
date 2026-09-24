# 🌊 ESP32 Flood Monitoring System

An ESP32-based flood monitoring system that monitors **temperature, humidity, rain, water level, and nearby objects** in real time.

The system uses an **ESP32**, water level sensor, rain sensor, DHT22, ultrasonic sensor, servo motor, and 16×2 I2C LCD to provide real-time environmental and flood-status information.

## 🚀 Features

* 🌡️ Real-time temperature monitoring
* 💧 Real-time humidity monitoring
* 🌧️ Rain detection
* 🌊 Water-level monitoring
* 📈 Water-level rising detection
* 📡 Ultrasonic object detection
* 🔄 Continuous 180° servo scanning
* 🚨 Flood warning detection
* 📺 16×2 I2C LCD display
* 🖥️ Serial Monitor output
* ⚡ ESP32-based real-time processing

## 🧰 Hardware Required

| Component                           |    Quantity |
| ----------------------------------- | ----------: |
| ESP32                               |           1 |
| 16×2 I2C LCD                        |           1 |
| DHT22 Temperature & Humidity Sensor |           1 |
| Rain Sensor                         |           1 |
| Water Level Sensor                  |           1 |
| HC-SR04 Ultrasonic Sensor           |           1 |
| Servo Motor                         |           1 |
| Jumper Wires                        | As required |
| Power Supply                        |           1 |

## 🔌 Pin Connections

| Component       | ESP32 Pin |
| --------------- | --------: |
| LCD SDA         |   GPIO 21 |
| LCD SCL         |   GPIO 22 |
| Servo Signal    |   GPIO 19 |
| Ultrasonic TRIG |    GPIO 5 |
| Ultrasonic ECHO |   GPIO 18 |
| DHT22 DATA      |    GPIO 4 |
| Water Sensor AO |   GPIO 35 |
| Rain Sensor AO  |   GPIO 34 |

### LCD Power

* VCC → 5V
* GND → GND
* SDA → GPIO 21
* SCL → GPIO 22

## 📺 LCD Display

The LCD cycles through the monitoring information:

```text
TEMP:24.0°C
HUM:70.0%
```

```text
RAIN STATUS
RAINING
```

```text
WATER LEVEL:
LEVEL HIGH
```

```text
FLOOD WARNING
FLOOD ALERT!
```

```text
OBJECT STATUS
DETECTED 15.2CM
```

```text
SERVO SCANNING
ANGLE:90°
```

The displayed values are obtained from the connected sensors.

## 🔄 Servo Operation

The servo continuously scans between:

```text
0° → 180° → 0°
```

The movement is controlled without blocking delays so that the sensors and LCD can continue operating while the servo moves.

## 🌧️ Rain Detection

The rain sensor is connected to **GPIO 34**.

The system compares the sensor's analog value against the configured threshold:

```cpp
#define RAIN_THRESHOLD 1800
```

The threshold may need to be adjusted according to the particular rain sensor.

## 🌊 Water-Level Detection

The water sensor is connected to **GPIO 35**.

The raw ADC value is converted to an approximate percentage:

```text
0 → 0%
4095 → 100%
```

The high-water threshold is currently:

```cpp
#define WATER_HIGH_THRESHOLD 2500
```

This value should be calibrated using the actual sensor.

## 📡 Ultrasonic Detection

The ultrasonic sensor measures the distance to nearby objects.

```text
TRIG → GPIO 5
ECHO → GPIO 18
```

Objects within the configured detection distance are reported as detected.

```cpp
#define OBJECT_DISTANCE 30.0
```

## 🚨 Flood Warning

A flood warning is generated when one or more of the following conditions occur:

* Water level is high
* Water level is increasing
* Rain is detected

The LCD displays:

```text
FLOOD WARNING
FLOOD ALERT!
```

when the warning condition is active.

## 🌡️ Temperature & Humidity

Temperature and humidity are measured using a DHT22 sensor.

```text
DHT22 DATA → GPIO 4
```

The code reads the sensor every 2 seconds.

## 💻 Software Requirements

Install the following Arduino IDE libraries:

* `LiquidCrystal_I2C`
* `DHT sensor library`
* `Adafruit Unified Sensor`
* `ESP32Servo`

Select an ESP32 board in the Arduino IDE before uploading.

## ⚙️ Setup

1. Connect all components according to the pin table.
2. Install the required Arduino libraries.
3. Open the `.ino` file in Arduino IDE.
4. Select your ESP32 board.
5. Select the correct COM port.
6. Upload the program.
7. Open Serial Monitor at **115200 baud**.
8. Observe the sensor readings and LCD status.

## 🖥️ Serial Monitor

The Serial Monitor displays information such as:

```text
========== FLOOD MONITOR ==========
Temperature: 24.0 C
Humidity: 70.0 %
Rain: RAINING
Rain Raw: 1200
Water Level: 75 %
Water Raw: 3072
Water Rising: YES
Distance: 15.4 cm
Object: DETECTED
Servo: 90 degrees
Flood: WARNING
===================================
```

## ⚠️ Important Safety Note

The ESP32 GPIO pins operate at **3.3 V logic**.

If using an HC-SR04 ultrasonic sensor powered at 5 V, the **ECHO signal should be reduced to a safe 3.3 V level** using an appropriate voltage divider before connecting it to the ESP32.

Also provide an adequate external power supply for the servo motor and connect its **GND to ESP32 GND**.

## 🔧 Calibration

Sensor thresholds depend on the individual sensors and installation.

The main values that may need adjustment are:

```cpp
#define RAIN_THRESHOLD 1800
```
