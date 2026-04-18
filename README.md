# 📡 Arduino Proximity Alert System (with Serial Monitoring)

A simple **Arduino-based proximity detection system** using an ultrasonic sensor, LEDs, a passive buzzer, and Serial Monitor feedback.

This project detects nearby objects and provides **visual, audio, and textual alerts** based on distance.

---

## 📌 Features

* 📏 Real-time distance measurement using ultrasonic sensing
* 🔴🟡🟢 LED indicators for proximity levels
* 🔊 Passive buzzer with variable pitch alerts
* 💻 Serial Monitor status messages
* ⚡ Lightweight and responsive logic

---

## 🧰 Components Required

* Arduino Uno (or compatible)
* Ultrasonic Sensor (HC-SR04 / PING)))
* Passive Buzzer
* 3 LEDs (Red, Yellow, Green)
* 220Ω resistors (for LEDs)
* Breadboard & jumper wires

---

## 🔌 Circuit Connections

### Ultrasonic Sensor

| Sensor Pin | Arduino Pin |
| ---------- | ----------- |
| VCC        | 5V          |
| GND        | GND         |
| TRIG       | 7           |
| ECHO       | 8           |

### LEDs

| LED Color | Arduino Pin |
| --------- | ----------- |
| Red       | 13          |
| Yellow    | 6           |
| Green     | 10          |

### Buzzer

| Component  | Arduino Pin |
| ---------- | ----------- |
| Buzzer (+) | 9           |
| Buzzer (-) | GND         |

---

## ⚙️ Working Principle

1. The ultrasonic sensor emits a sound pulse.
2. The echo return time is measured using `pulseIn()`.
3. Distance is calculated:

```cpp
cm = duration / 29 / 2;
```

4. System response based on distance:

* **< 10 cm**
  → 🔴 Red LED
  → 🔊 High-pitch buzzer
  → 💻 `"Obstacle in contact!"`

* **10–30 cm**
  → 🟡 Yellow LED
  → 🔊 Medium-pitch buzzer
  → 💻 `"Obstacle nearby!"`

* **> 30 cm**
  → 🟢 Green LED
  → 🔇 No sound
  → 💻 `"All clear!"`

---

## 💻 Code

```cpp
const int trigPin = 7;
const int echoPin = 8;
const int buzzerPin = 9;
const int redLedPin = 13;
const int yellowLedPin = 6;
const int greenLedPin = 10;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(redLedPin, OUTPUT);
  pinMode(yellowLedPin, OUTPUT);
  pinMode(greenLedPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  long duration, cm;

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  cm = duration / 29 / 2;

  digitalWrite(redLedPin, LOW);
  digitalWrite(yellowLedPin, LOW);
  digitalWrite(greenLedPin, LOW);

  if (cm < 10) {
    digitalWrite(redLedPin, HIGH);
    tone(buzzerPin, 1000);
    Serial.println("Obstacle in contact!");
  } 
  else if (cm < 30) {
    digitalWrite(yellowLedPin, HIGH);
    tone(buzzerPin, 500);
    Serial.println("Obstacle nearby!");
  } 
  else {
    digitalWrite(greenLedPin, HIGH);
    noTone(buzzerPin);
    Serial.println("All clear!");
    delay(60);
  }

  delay(100);
}
```

---

## 🖥️ Serial Monitor Output

Set baud rate to **9600**:

```
Obstacle in contact!
Obstacle nearby!
All clear!
```

---

## 🎯 Behavior Summary

| Distance | LED | Buzzer | Status  |
| -------- | --- | ------ | ------- |
| < 10 cm  | 🔴  | High   | Contact |
| 10–30 cm | 🟡  | Medium | Nearby  |
| > 30 cm  | 🟢  | Off    | Clear   |

---

## 🚀 Possible Applications

* Obstacle detection for robots
* Smart alert system for blind assistance
* Intruder proximity alert
* Distance monitoring demo project
* Interactive installations

---

## 🧠 Learning Outcomes

* Ultrasonic sensing principles
* Distance calculation using time-of-flight
* Digital I/O handling in Arduino
* Generating sound using `tone()`
* Serial communication for debugging

---

## 📄 License

Free to use for educational and personal projects.

---

⭐ You can extend this with LCD, IoT modules, or smarter alert logic.
