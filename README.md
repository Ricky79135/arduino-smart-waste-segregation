# arduino-smart-waste-segregation
Arduino-based automatic waste segregation system that classifies waste into wet, dry, and metal categories using sensors and motorized sorting mechanisms.

Topics/tags:

arduino, iot, embedded-systems, waste-segregation, smart-dustbin, robotics, sensors, automation, arduino-uno

# ♻️ **Arduino-Based Smart Waste Segregation System**

An Arduino-based automatic waste segregation system designed to identify and separate **wet, dry, and metal waste** using sensors and motorized mechanisms.

The system detects incoming waste, analyzes its physical properties using multiple sensors, classifies it into the appropriate category, and automatically directs it into the corresponding collection compartment.

---

## 📌 Project Overview

Waste segregation at the source is an important step toward efficient recycling and sustainable waste management.

This project demonstrates a low-cost automated waste segregation prototype using an **Arduino UNO**, **IR sensor**, **metal sensor**, **moisture sensor**, **servo motor**, **stepper motor**, and **ULN2003 motor driver**.

The system follows a sensor-based decision-making approach:

```text
              Waste Input
                   │
                   ▼
              IR Sensor
                   │
                   ▼
           Object Detected
                   │
                   ▼
            Metal Detection
             /           \
           YES             NO
            │               │
            ▼               ▼
        METAL BIN      Moisture Detection
                         /          \
                       WET          DRY
                        │            │
                        ▼            ▼
                     WET BIN      DRY BIN
```
                 ┌───────────────┐
                 │  ARDUINO UNO  │
                 └───────┬───────┘
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
       ▼                 ▼                 ▼
   IR SENSOR       METAL SENSOR      MOISTURE SENSOR
       │                 │                 │
       └─────────────────┼─────────────────┘
                         │
                         ▼
                   DECISION LOGIC
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
        SERVO MOTOR          ULN2003 DRIVER
                                    │
                                    ▼
                              STEPPER MOTOR
                                    │
                                    ▼
                            SORTING MECHANISM
---

## 🎯 Objectives

* Automatically detect incoming waste.
* Identify metallic waste.
* Distinguish between wet and dry waste.
* Automatically move the sorting mechanism.
* Reduce the need for manual waste segregation.
* Demonstrate the application of embedded systems and automation in waste management.

---

## ⚙️ Technologies Used

* Arduino UNO
* Arduino IDE
* Embedded C/C++
* IR Sensor
* Metal Detection Sensor
* Moisture Sensor
* Servo Motor
* 28BYJ-48 Stepper Motor
* ULN2003 Stepper Motor Driver
* DC Power Supply
* Jumper Wires
* Custom Mechanical Structure

---

🔄 System Architecture
                    ┌─────────────────┐
                    │   Waste Input   │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │    IR Sensor    │
                    │ Object Detection│
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │   Arduino UNO   │
                    │  Control Logic  │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │  Metal Sensor   │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │Moisture Sensor  │
                    └────────┬────────┘
                             ↓
                     Classification
                             │
             ┌───────────────┼───────────────┐
             ↓               ↓               ↓
          METAL             WET             DRY
             │               │               │
             └───────────────┼───────────────┘
                             ↓
                    Motorized Mechanism
                             ↓
                    ┌─────────────────┐
                    │ Waste Collection│
                    │   Compartments  │
                    └─────────────────┘
---

## 🔧 Hardware Components

| Component               | Purpose                             |
| ----------------------- | ----------------------------------- |
| Arduino UNO             | Main controller                     |
| IR Sensor               | Detects the presence of waste       |
| Metal Sensor            | Detects metallic material           |
| Moisture Sensor         | Determines wet/dry condition        |
| Servo Motor             | Controls the mechanical gate/action |
| 28BYJ-48 Stepper Motor  | Positions the sorting mechanism     |
| ULN2003 Driver          | Drives the stepper motor            |
| Power Supply            | Provides power to the system        |
| Collection Compartments | Stores separated waste              |

---

## 🧠 Working Principle

The system works in the following sequence:

### 1. Waste Detection

The IR sensor detects when an object is placed into the input section.

### 2. Metal Detection

The system checks whether the object contains metal.

If metal is detected, the waste is classified as:

**METAL**

and directed to the metal compartment.

### 3. Moisture Detection

If metal is not detected, the moisture sensor measures the moisture level of the object.

The Arduino compares the sensor reading with a predefined threshold.

* High moisture → **WET**
* Low moisture → **DRY**

### 4. Motorized Sorting

The Arduino controls the servo and stepper motor to position the sorting mechanism over the appropriate compartment.

### 5. Waste Collection

The waste falls into its corresponding compartment.

---

## 🔄 Classification Logic

```text
IF waste is detected
    |
    ├── IF metal detected
    |       └──> METAL
    |
    └── ELSE
            |
            ├── IF moisture is above threshold
            |       └──> WET
            |
            └── ELSE
                    └──> DRY
```

---



---

## 🚀 How to Run the Project

### Step 1 — Install Arduino IDE

Install the Arduino IDE on your computer.

### Step 2 — Connect the Hardware

Connect the sensors, motors, and motor driver according to the circuit diagram provided in this repository.

### Step 3 — Open the Code

Open:

```text#include <CheapStepper.h>

#include <Servo.h>
Servo servo1;
#define ir 5
#define proxi 6
#define buzzer 12
int potPin = A0; //input pin
int soil=0;
int fsoil;

CheapStepper stepper (8,9,10,11);

void setup()
{Serial.begin(9600);
  pinMode(proxi, INPUT_PULLUP);
  pinMode(ir, INPUT);
  pinMode(buzzer, OUTPUT);
  servo1.attach(7);
  stepper.setRpm(17); 
  servo1.write(180);
delay(1000);
servo1.write(70);
delay(1000);
  
}

void loop() 
{
  fsoil=0;
  int L =digitalRead(proxi);
Serial.print(L);
if(L==0)
{
  tone(buzzer, 1000, 1000);
 stepper.moveDegreesCW (240);
delay(1000); 
servo1.write(180);
delay(1000);
servo1.write(70);
delay(1000);
stepper.moveDegreesCCW (240);
delay(1000); 
} 

if(digitalRead(ir)==0)
{
   tone(buzzer, 1000, 500);
   delay(1000);
  int soil=0;
  for(int i=0;i<3;i++)
    {
    soil = analogRead(potPin) ;
      soil = constrain(soil, 485, 1023);
        fsoil = (map(soil, 485, 1023, 100, 0))+fsoil;
          delay(75);
      }
    fsoil=fsoil/3;
    Serial.print(fsoil);
    Serial.print("%");Serial.print("\n");

    if(fsoil>20)
           {
            stepper.moveDegreesCW (120);
            delay(1000); 
              servo1.write(180);
              delay(1000);
              servo1.write(70);
                delay(1000);
              stepper.moveDegreesCCW (120);
                delay(1000); 
              } 

      else {
         tone(buzzer, 1000, 500);
           delay(1000);
            servo1.write(180);
              delay(1000);
              servo1.write(70);
                delay(1000);}
}

}

```

in Arduino IDE.

### Step 4 — Select the Board

Select:

```text
Arduino UNO 
```

from:

```text
Tools → Board
```

### Step 5 — Select the COM Port

Select the appropriate Arduino COM port.

### Step 6 — Upload

Compile and upload the program to the Arduino UNO.

### Step 7 — Test

Place different types of waste into the input section and observe the automatic segregation process.

---
⚙️ How the System Works

The system operates through a sequence of detection, classification, and mechanical sorting.

1. Waste Detection

When an object is placed into the input section, the IR sensor detects its presence.

The Arduino continuously monitors the sensor and starts the classification process when an object is detected.

Waste placed
     ↓
IR Sensor
     ↓
Object detected
     ↓
Classification begins

2. Metal Detection

After detecting an object, the system checks the metal detection sensor.

If metallic material is detected, the Arduino classifies the object as Metal Waste.

Metal detected
      ↓
METAL
      ↓
Metal compartment

3. Moisture Detection

If metal is not detected, the Arduino checks the moisture sensor.

The moisture reading is compared against a predefined threshold.

No metal detected
       ↓
Moisture sensor
       ↓
 ┌─────┴─────┐
 ↓           ↓
Wet         Dry
 ↓           ↓
Wet Bin     Dry Bin

4. Mechanical Sorting

Once the category has been determined, the Arduino activates the motorized mechanism.

The stepper motor positions the sorting mechanism, while the servo motor can control the opening, closing, or movement of the mechanical gate depending on the design.

5. Waste Collection

The waste is directed into the appropriate compartment:

Metal → Metal compartment
Wet → Wet compartment
Dry → Dry compartment

After sorting, the mechanism returns to its initial position and waits for the next object.
---

## 📊 Waste Categories

| Category | Detection Method                      |
| -------- | ------------------------------------- |
| 🥫 Metal | Metal sensor                          |
| 💧 Wet   | Moisture sensor                       |
| 📄 Dry   | Moisture threshold + absence of metal |

---

## ⚠️ Limitations

This project is a prototype and uses sensor-based classification.

The system does not use computer vision or machine learning. Classification depends on sensor readings and calibrated thresholds.

Possible limitations include:

* Incorrect classification of mixed-material objects
* Moisture sensor calibration requirements
* Limited metal detection range
* Mechanical jams
* Sensor noise
* Power requirements of motors

---

## 🔮 Future Improvements

The project can be enhanced by adding:

* ESP32-based IoT connectivity
* Real-time waste-bin monitoring
* Ultrasonic sensors for bin-level detection
* LCD/OLED display
* Buzzer notifications
* Mobile/web dashboard
* Cloud data storage
* Camera-based waste recognition
* Machine learning classification
* Automatic bin-full alerts
* Solar-powered operation

---
⚙️ Motor Control

The project uses motorized components to convert the Arduino's classification decision into a physical sorting action.

Stepper Motor

The stepper motor provides controlled rotational movement and allows the sorting mechanism to be positioned according to the detected waste category.

Servo Motor

The servo motor provides controlled angular movement and can be used to operate the mechanical gate or waste-release mechanism.

ULN2003 Driver

The ULN2003 driver module is used as an interface between the Arduino and the stepper motor, allowing the Arduino to control the motor's coils safely and reliably.
---

## 🎓 Learning Outcomes

Through this project, I gained practical experience in:

* Arduino programming
* Embedded systems
* Sensor integration
* Motor control
* Hardware interfacing
* Automation
* Embedded C/C++
* Troubleshooting
* Mechanical-electronic system integration
* Waste management automation


## ⭐ Acknowledgements

We sincerely thank our faculty, mentors, and institution for their valuable guidance and support. As a team, we appreciate every member’s dedication, teamwork, and contribution. As Team Leader, I am proud of our team for successfully designing, developing, and completing this project.
