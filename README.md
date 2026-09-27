# 🔋 Integrated ESP32 Battery Management System

## Production-Grade Battery Intelligence, Safety, Fault Tolerance & IoT Monitoring

An integrated **ESP32-based Battery Management System (BMS)** developed as a unified embedded-system project.

The system monitors a simulated **4-cell lithium-ion battery pack**, calculates battery intelligence parameters, detects safety and runtime faults, provides a local LCD diagnostic interface, controls protection hardware, communicates with Blynk IoT, and presents battery information through an executive dashboard.

> **All six challenge tasks are integrated and demonstrated within one single Wokwi project.** The project is designed as one complete end-to-end BMS solution rather than six independent submissions.

---

# 📌 Six Integrated Challenge Tasks

| Task   | Module                                          | Main Function                           |
| ------ | ----------------------------------------------- | --------------------------------------- |
| Task 1 | Adaptive Multi-Cell Battery Intelligence Engine | Cell monitoring, SOC, imbalance, health |
| Task 2 | Event-Driven Safety Protection Kernel           | Fault detection and protection          |
| Task 3 | Intelligent Embedded HMI                        | LCD diagnostics and fault display       |
| Task 4 | Fault-Tolerant Embedded Runtime System          | Fault handling and recovery             |
| Task 5 | Intelligent Cloud Telemetry Architecture        | Blynk IoT and event telemetry           |
| Task 6 | Executive Battery Intelligence Dashboard        | Live analytics and operator information |

---

# 🏗️ Complete System Architecture

```text
                  4 SIMULATED BATTERY CELLS
                           │
                           ▼
                  ┌───────────────────┐
                  │ ESP32 ADC ENGINE  │
                  └─────────┬─────────┘
                            │
                            ▼
             ┌────────────────────────────┐
             │ TASK 1                     │
             │ BATTERY INTELLIGENCE      │
             │                            │
             │ Cell Voltage              │
             │ Pack Voltage              │
             │ Average Voltage            │
             │ SOC                        │
             │ Imbalance                  │
             │ Weakest / Strongest Cell   │
             │ Health Classification      │
             └──────────────┬─────────────┘
                            │
                            ▼
             ┌────────────────────────────┐
             │ TASK 4                     │
             │ FAULT-TOLERANT RUNTIME     │
             │                            │
             │ NORMAL                     │
             │ DEGRADED                   │
             │ FAILSAFE                   │
             │ SHUTDOWN                   │
             └──────────────┬─────────────┘
                            │
                            ▼
             ┌────────────────────────────┐
             │ TASK 2                     │
             │ SAFETY PROTECTION          │
             │                            │
             │ UV / OV                    │
             │ dV/dt                      │
             │ Sensor Fault               │
             │ Relay Protection           │
             │ Buzzer                     │
             └──────────────┬─────────────┘
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
      ┌────────────────┐          ┌──────────────────┐
      │ TASK 3         │          │ TASK 5           │
      │ LCD HMI        │          │ CLOUD TELEMETRY  │
      │                │          │                  │
      │ Diagnostics    │          │ Blynk            │
      │ Fault Display  │          │ Wi-Fi            │
      │ Status         │          │ Event Queue      │
      └───────┬────────┘          └────────┬─────────┘
              │                            │
              └─────────────┬──────────────┘
                            ▼
                  ┌────────────────────┐
                  │ TASK 6             │
                  │ EXECUTIVE          │
                  │ DASHBOARD          │
                  │                    │
                  │ Live Data          │
                  │ Risk               │
                  │ Faults             │
                  │ Recommendations    │
                  └────────────────────┘
```

---

# 🔌 Hardware Components

The Wokwi prototype uses:

* ESP32 DevKit V1
* 4 × 10 kΩ potentiometers
* 16×2 I2C LCD
* PCF8574 I2C backpack
* 1 × relay module
* 1 × active buzzer
* 1 × red LED
* 1 × yellow LED
* 1 × green LED
* Push button for screen control
* Push button for fault injection
* Relay feedback signal
* Blynk IoT cloud

The potentiometers represent the four simulated lithium-ion cells.

---

# 📍 COMPLETE PIN CONFIGURATION

> **Important:** This is the final conflict-free pin map. Do not assign the same GPIO to two different components.

| ESP32 GPIO | Component      | Function                  | Direction |
| ---------: | -------------- | ------------------------- | --------- |
|    GPIO 34 | POT 1          | Cell 1 ADC                | INPUT     |
|    GPIO 35 | POT 2          | Cell 2 ADC                | INPUT     |
|    GPIO 32 | POT 3          | Cell 3 ADC                | INPUT     |
|    GPIO 33 | POT 4          | Cell 4 ADC                | INPUT     |
|    GPIO 21 | LCD            | I2C SDA                   | I/O       |
|    GPIO 22 | LCD            | I2C SCL                   | I/O       |
|    GPIO 13 | Relay          | Relay command             | OUTPUT    |
|    GPIO 14 | Buzzer         | Fault buzzer              | OUTPUT    |
|    GPIO 25 | Red LED        | Critical/Fault            | OUTPUT    |
|    GPIO 26 | Yellow LED     | Warning/Imbalance         | OUTPUT    |
|    GPIO 27 | Green LED      | Healthy                   | OUTPUT    |
|    GPIO 18 | Push Button    | HMI screen control        | INPUT     |
|    GPIO 19 | Relay feedback | Relay status verification | INPUT     |
|    GPIO 23 | Push Button    | Fault injection/demo      | INPUT     |

### ⚠️ ESP32 ADC Note

GPIO 34 and GPIO 35 are input-only pins, which makes them suitable for analog sensor inputs.

GPIO 32 and GPIO 33 are also used as ADC inputs.

---

# 🔧 WOKWI WIRING INSTRUCTIONS

## 1. Cell 1 Potentiometer

```text
POT 1
 ├── VCC → 3.3V
 ├── GND → GND
 └── SIG → GPIO 34
```

## 2. Cell 2 Potentiometer

```text
POT 2
 ├── VCC → 3.3V
 ├── GND → GND
 └── SIG → GPIO 35
```

## 3. Cell 3 Potentiometer

```text
POT 3
 ├── VCC → 3.3V
 ├── GND → GND
 └── SIG → GPIO 32
```

## 4. Cell 4 Potentiometer

```text
POT 4
 ├── VCC → 3.3V
 ├── GND → GND
 └── SIG → GPIO 33
```

---

# 📺 LCD CONNECTION

For the 16×2 I2C LCD:

```text
LCD VCC → ESP32 5V/VIN
LCD GND → ESP32 GND
LCD SDA → GPIO 21
LCD SCL → GPIO 22
```

I2C address:

```text
0x27
```

Arduino initialization:

```cpp
LiquidCrystal_I2C lcd(0x27, 16, 2);
```

---

# 🔴🟡🟢 LED CONNECTIONS

## Red LED

```text
ESP32 GPIO 25
      │
      └── 220Ω resistor ──► LED ──► GND
```

Function:

```text
FAULT / CRITICAL
```

## Yellow LED

```text
ESP32 GPIO 26
      │
      └── 220Ω resistor ──► LED ──► GND
```

Function:

```text
WARNING / IMBALANCE
```

## Green LED

```text
ESP32 GPIO 27
      │
      └── 220Ω resistor ──► LED ──► GND
```

Function:

```text
HEALTHY
```

---

# 🔊 BUZZER

```text
Buzzer +
   │
   └── GPIO 14

Buzzer -
   │
   └── GND
```

The buzzer is activated during critical safety events.

---

# 🔌 RELAY

```text
Relay IN → GPIO 13
Relay VCC → 5V/VIN
Relay GND → GND
```

The relay is used as the simulated battery protection cutoff.

### Relay logic

```text
NORMAL
   ↓
Relay ON

CRITICAL FAULT
   ↓
Relay OFF
```

---

# 🔄 RELAY FEEDBACK

The relay feedback signal is connected to:

```text
GPIO 19
```

This allows the runtime subsystem to compare:

```text
Commanded relay state
        vs
Actual relay feedback
```

A mismatch can generate a runtime fault.

---

# 🔘 BUTTONS

## HMI Button

```text
GPIO 18 → Push Button → GND
```

Used for manual HMI/screen control.

Configure with:

```cpp
pinMode(BUTTON_PIN, INPUT_PULLUP);
```

## Fault Injection Button

```text
GPIO 23 → Push Button → GND
```

Used for demonstrating fault-tolerant runtime behavior.

---

# 📚 LIBRARIES

Create a file named:

```text
libraries.txt
```

It must contain **only**:

```text
Blynk
LiquidCrystal I2C
```

Do not put:

```text
sketch.ino
diagram.json
```

inside `libraries.txt`.

---

# 📁 PROJECT STRUCTURE

```text
battery-safety-monitoring-systems/
│
├── sketch.ino
├── diagram.json
├── libraries.txt
└── README.md
```

---

# 💻 MAIN SOFTWARE STRUCTURE

The firmware is organized into independent modules.

```text
setup()
 │
 ├── GPIO initialization
 ├── ADC configuration
 ├── LCD initialization
 ├── Wi-Fi initialization
 ├── Blynk initialization
 ├── safety initialization
 └── runtime initialization
       │
       ▼
loop()
 │
 ├── readCells()
 ├── calculateBatteryAnalytics()
 ├── updateHealthState()
 ├── runSafetyProtection()
 ├── runFaultRuntime()
 ├── updateHMI()
 ├── processTelemetry()
 ├── processEventQueue()
 ├── updateDashboard()
 └── processButtons()
```

---

# 🧮 TASK 1 — BATTERY INTELLIGENCE ENGINE

## ADC Sampling

Each cell is sampled multiple times to reduce ADC noise.

Example:

```cpp
uint32_t adcSum = 0;

for (int i = 0; i < 8; i++) {
    adcSum += analogRead(cellPins[cell]);
}

float adcAverage = adcSum / 8.0;
```

The implementation uses:

```text
12-bit ADC
0–4095
```

and oversampling.

---

# 🔋 CELL VOLTAGE CALCULATION

The simulated ADC value is converted into a modeled cell voltage.

```cpp
float cellVoltageFromADC(int raw)
{
    float adcVoltage = (raw / 4095.0) * 3.3;

    float cellVoltage =
        2.5 + (adcVoltage / 3.3) * (4.2 - 2.5);

    return constrain(cellVoltage, 2.5, 4.2);
}
```

> This is a simulation model. A real BMS would use an appropriate cell-sensing front end rather than directly connecting a lithium cell to an ESP32 ADC.

---

# 📊 PACK ANALYTICS

Pack voltage:

```cpp
packVoltage =
    cell1 +
    cell2 +
    cell3 +
    cell4;
```

Average cell voltage:

```cpp
averageVoltage =
    packVoltage / 4.0;
```

Delta voltage:

```cpp
deltaVoltage =
    maximumCell - minimumCell;
```

---

# ⚖️ CELL IMBALANCE

Individual cell imbalance:

```text
|Vcell − Vavg|
---------------- × 100
       Vavg
```

Pack imbalance:

```text
(Vmax − Vmin)
------------- × 100
     Vavg
```

Example:

```cpp
float imbalance =
    ((maxCell - minCell) / averageVoltage) * 100.0;
```

---

# 🔋 SOC ESTIMATION

The prototype uses a linear voltage-based SOC model.

```cpp
float calculateSOC(float voltage)
{
    float soc =
        ((voltage - 2.5) / (4.2 - 2.5)) * 100.0;

    return constrain(soc, 0.0, 100.0);
}
```

Reference:

```text
2.50 V → 0%
4.20 V → 100%
```

This is a demonstration model and not a substitute for a chemistry-specific SOC estimator.

---

# 🩺 HEALTH CLASSIFICATION

```text
             Battery Analytics
                    │
                    ▼
             Any critical fault?
                /          \
              YES           NO
              │              │
              ▼              ▼
        PACK FAILURE      Imbalance?
                         /          \
                       >10%        >3%
                        │            │
                        ▼            ▼
                  CRITICAL       MINOR
                  IMBALANCE      IMBALANCE
                                    │
                                    ▼
                                  HEALTHY
```

---

# 🛡️ TASK 2 — SAFETY PROTECTION

## Safety thresholds

```cpp
#define UV_FAULT       2.80
#define OV_FAULT       4.15
#define MIN_VOLTAGE    2.50
#define MAX_VOLTAGE    4.20
#define IMBALANCE_WARN 3.0
#define IMBALANCE_CRIT 10.0
#define DV_DT_LIMIT    0.15
```

---

# ⚠️ FAULT DETECTION

The safety kernel checks:

```text
1. Undervoltage
2. Overvoltage
3. Rapid voltage change
4. Sensor anomaly
5. Relay mismatch
6. Invalid ADC reading
```

---

# ⏱️ NON-BLOCKING FAULT DEBOUNCE

A fault is not immediately accepted from one noisy sample.

Conceptually:

```cpp
if (faultCondition) {

    if (faultStartTime == 0)
        faultStartTime = millis();

    if (millis() - faultStartTime >= DEBOUNCE_TIME)
        confirmFault();

} else {

    faultStartTime = 0;
}
```

This avoids false triggering.

---

# 🔄 SAFETY STATE MACHINE

```text
             ┌──────────────┐
             │    NORMAL    │
             └──────┬───────┘
                    │
                 Fault
                    ▼
             ┌──────────────┐
             │    FAULT     │
             └──────┬───────┘
                    │
              Fault removed
                    ▼
             ┌──────────────┐
             │   COOLDOWN   │
             └──────┬───────┘
                    │
              Stable system
                    ▼
             ┌──────────────┐
             │    NORMAL    │
             └──────────────┘
```

---

# 🧯 TASK 4 — FAULT-TOLERANT RUNTIME

The runtime subsystem uses four operational modes:

```cpp
enum RuntimeMode {
    NORMAL,
    DEGRADED,
    FAILSAFE,
    SHUTDOWN
};
```

### NORMAL

All monitored subsystems are operating normally.

### DEGRADED

A non-critical fault exists while essential monitoring remains operational.

### FAILSAFE

A serious fault has been detected and protection is activated.

Typical actions:

```text
Relay OFF
Buzzer ON
Red LED ON
Fault logged
Cloud event generated
```

### SHUTDOWN

Persistent or severe faults force the system into a safe terminal operating state until explicit recovery conditions are met.

---

# 🧾 FAULT LOGGING

A structured event contains:

```cpp
struct FaultEvent {
    unsigned long timestamp;
    String type;
    String severity;
    int cell;
    float value;
};
```

Example:

```text
5230 ms
UNDERVOLTAGE
CRITICAL
CELL 2
2.74 V
```

---

# 🖥️ TASK 3 — LCD HMI

The LCD automatically rotates through diagnostic screens.

## Screen 1

```text
PACK 14.80V
SOC 71% HEALTHY
```

## Screen 2

```text
C1:3.70 C2:3.71
W:C1 S:C2
```

## Screen 3

```text
C3:3.69 C4:3.70
dV:0.020V
```

## Screen 4

```text
IMB:0.5%
RLY:ON
```

---

# 🚨 FAULT OVERRIDE

During a critical fault:

```text
Normal Screen
      ↓
Fault detected
      ↓
Immediate LCD override
      ↓
FAULT / WARNING
      ↓
Fault resolved
      ↓
Normal rotation resumes
```

---

# ☁️ TASK 5 — BLYNK CLOUD TELEMETRY

The Blynk library is included using:

```cpp
#include <BlynkSimpleEsp32.h>
```

Blynk credentials:

```cpp
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "ESP32 Battery BMS"
#define BLYNK_AUTH_TOKEN "YOUR_BLYNK_TOKEN"
```

---

# 📡 EVENT-DRIVEN TELEMETRY

Telemetry is generated when meaningful changes occur.

Examples:

```text
Health changed
SOC changed significantly
Imbalance changed significantly
Cell voltage changed significantly
Fault detected
Threshold crossed
Wi-Fi reconnects
```

This reduces unnecessary network traffic compared with blindly transmitting every sensor sample.

---

# 📥 OFFLINE EVENT QUEUE

During network failure:

```text
Sensor
  ↓
Battery Event
  ↓
Wi-Fi unavailable
  ↓
20-event Ring Buffer
  ↓
Wi-Fi reconnect
  ↓
Queue synchronization
  ↓
Blynk Cloud
```

Maximum queue capacity:

```text
20 events
```

---

# 📶 RSSI MONITORING

Wi-Fi signal strength is monitored using:

```cpp
WiFi.RSSI()
```

Example classification:

```text
Excellent
Good
Fair
Weak
```

---

# 📊 TASK 6 — EXECUTIVE DASHBOARD

## Blynk Virtual Pins

| Pin | Data                    | Widget   |
| --- | ----------------------- | -------- |
| V0  | Cell 1 voltage          | Gauge    |
| V1  | Cell 2 voltage          | Gauge    |
| V2  | Cell 3 voltage          | Gauge    |
| V3  | Cell 4 voltage          | Gauge    |
| V4  | Pack voltage            | Gauge    |
| V5  | SOC                     | Gauge    |
| V6  | Imbalance               | Gauge    |
| V7  | Health                  | Value    |
| V8  | RSSI                    | Value    |
| V9  | Event log               | Terminal |
| V10 | Risk level              | Value    |
| V11 | Operator recommendation | Value    |
| V12 | Weakest cell            | Value    |
| V13 | Strongest cell          | Value    |
| V14 | Green indicator         | LED      |
| V15 | Yellow indicator        | LED      |
| V16 | Red indicator           | LED      |

---

# 🟢🟡🔴 SEVERITY INDICATION

```text
GREEN
System healthy
       ↓
YELLOW
Warning / imbalance
       ↓
RED
Critical fault
```

---

# 🧠 OPERATOR RECOMMENDATIONS

The system generates actionable messages based on battery conditions.

Examples:

```text
SYSTEM NOMINAL
```

```text
MONITOR IMBALANCE
```

```text
BALANCE CELLS
```

```text
CHARGE SOON
```

```text
STOP - LOW CELL
```

```text
STOP - OVERVOLTAGE
```

---

# 📈 HISTORICAL TREND

The firmware maintains a rolling voltage history for selected battery parameters.

Conceptually:

```text
Voltage
  │
4.2│       ●
   │      / \
3.7│ ●───●   ●───●
   │
3.2│
   └──────────────────► Time
```

This allows the dashboard architecture to be extended for voltage-trend visualization.

---

# ⏱️ TIMING ARCHITECTURE

The system avoids blocking `delay()` calls in the main runtime.

Example scheduler:

| Module                |     Interval |
| --------------------- | -----------: |
| ADC sampling          |       100 ms |
| Safety evaluation     |       100 ms |
| Runtime monitoring    |       100 ms |
| HMI update            |       100 ms |
| LCD screen rotation   |          4 s |
| Wi-Fi retry           |         10 s |
| Cloud events          | Event driven |
| Queue synchronization | On reconnect |

The implementation uses:

```cpp
millis()
```

for task scheduling.

---

# 🧪 WOKWI DEMONSTRATION PROCEDURE

## Test 1 — Healthy Battery

Set all four potentiometers approximately equal:

```text
C1 ≈ 3.70 V
C2 ≈ 3.70 V
C3 ≈ 3.70 V
C4 ≈ 3.70 V
```

Expected:

```text
Health: HEALTHY
LED: GREEN
Relay: ON
Buzzer: OFF
```

---

## Test 2 — Minor Imbalance

Create a small cell difference.

Example:

```text
C1 = 3.70 V
C2 = 3.70 V
C3 = 3.70 V
C4 = 3.50 V
```

Expected:

```text
Health: MINOR IMBALANCE
LED: YELLOW
Recommendation: MONITOR / BALANCE
```

---

## Test 3 — Critical Imbalance

Create a larger difference.

Expected:

```text
Health: CRITICAL IMBALANCE
LED: RED
Risk: HIGH
Recommendation: BALANCE CELLS
```

---

## Test 4 — Undervoltage

Set one cell below:

```text
2.80 V
```

Expected:

```text
FAULT
Relay OFF
Red LED ON
Buzzer ON
LCD warning
Fault log
Cloud event
```

---

## Test 5 — Overvoltage

Set one cell above:

```text
4.15 V
```

Expected:

```text
OVERVOLTAGE
Relay OFF
Red LED ON
Buzzer ON
LCD warning
Cloud event
```

---

## Test 6 — Sensor Fault

Use the fault-injection mechanism to simulate an invalid or frozen sensor.

Expected:

```text
DEGRADED
       ↓
FAILSAFE
```

depending on fault severity and persistence.

---

## Test 7 — Wi-Fi Failure

Disconnect or disable the network.

Expected:

```text
Wi-Fi LOST
     ↓
Local BMS continues
     ↓
Events stored
     ↓
Wi-Fi reconnect
     ↓
Queued events synchronized
```

---

# 🧮 BATTERY THRESHOLD REFERENCE

| Parameter               |      Value |
| ----------------------- | ---------: |
| Minimum modeled voltage |     2.50 V |
| Undervoltage fault      |     2.80 V |
| Nominal reference       |     3.70 V |
| Overvoltage fault       |     4.15 V |
| Maximum modeled voltage |     4.20 V |
| Minor imbalance         |       > 3% |
| Critical imbalance      |      > 10% |
| Rapid dV/dt threshold   | > 0.15 V/s |
| ADC resolution          |     12-bit |
| ADC range               |     0–4095 |
| ADC oversampling        |  8 samples |

---

# 🔐 SAFETY NOTE

This project is an **educational Wokwi prototype**.

The simulated voltage conversion, SOC estimation, thresholds, relay protection and fault models are intended for demonstrating embedded-system architecture.

They should **not be directly used as a real EV, aircraft, industrial battery or safety-critical BMS** without appropriate battery-monitoring ICs, isolation, redundant sensing, thermal monitoring, balancing circuitry, validated protection logic, EMC testing and applicable safety certification.

---

# 🛠️ TECHNOLOGY STACK

```text
Microcontroller : ESP32 DevKit V1
Language        : C++
Framework       : Arduino ESP32
Simulation      : Wokwi
Cloud           : Blynk IoT
Display         : 16×2 I2C LCD
Communication   : Wi-Fi
ADC             : ESP32 12-bit ADC
Timing          : millis()
Architecture    : Event-driven / modular
```

---

# 📂 FINAL PROJECT FILES

```text
battery-safety-monitoring-systems/
│
├── sketch.ino
│      └── Complete integrated ESP32 firmware
│
├── diagram.json
│      └── Complete Wokwi hardware wiring
│
├── libraries.txt
│      └── Required libraries
│
└── README.md
       └── Complete project documentation
```

---

# 📋 QUICK START

### Step 1

Open the Wokwi project.

### Step 2

Verify:

```text
sketch.ino
diagram.json
libraries.txt
```

### Step 3

Make sure `libraries.txt` contains:

```text
Blynk
LiquidCrystal I2C
```

### Step 4

Configure Blynk credentials in `sketch.ino`.

### Step 5

Start Wokwi simulation.

### Step 6

Adjust the four potentiometers to demonstrate different battery conditions.

### Step 7

Observe:

```text
LCD
LEDs
Relay
Buzzer
Serial Monitor
Blynk Dashboard
```

---

# 🎓 Skills Demonstrated

This project demonstrates practical knowledge of:

* ESP32
* Embedded C++
* ADC interfacing
* Battery monitoring
* Battery analytics
* SOC estimation
* Cell balancing analysis
* State machines
* Event-driven programming
* Non-blocking firmware
* `millis()` scheduling
* Fault detection
* Fault recovery
* Fault logging
* Relay protection
* Buzzer control
* LED diagnostics
* LCD HMI
* I2C communication
* Wi-Fi communication
* IoT telemetry
* Blynk
* Cloud dashboards
* Wokwi simulation
* Modular embedded architecture

---

# 🏁 END-TO-END SYSTEM FLOW

```text
        SENSE
          ↓
     ADC SAMPLING
          ↓
       ANALYZE
          ↓
   CELL INTELLIGENCE
          ↓
       DETECT
          ↓
    FAULT ANALYSIS
          ↓
      PROTECT
          ↓
  RELAY / BUZZER / LED
          ↓
      DISPLAY
          ↓
        LCD HMI
          ↓
      TRANSMIT
          ↓
    BLYNK CLOUD
          ↓
     VISUALIZE
          ↓
 EXECUTIVE DASHBOARD
          ↓
       RECOVER
          ↓
    NORMAL OPERATION
```

---

# ✅ PROJECT STATUS

**Integrated six-task Battery Management System**

```text
Task 1  ✓ Battery Intelligence
Task 2  ✓ Safety Protection
Task 3  ✓ Embedded HMI
Task 4  ✓ Fault-Tolerant Runtime
Task 5  ✓ Cloud Telemetry
Task 6  ✓ Executive Dashboard

Platform: ESP32 + Wokwi
Architecture: Unified Integrated System
```

> **All six challenges are implemented as interconnected modules within a single Wokwi project and are intended to be demonstrated together as one complete Battery Management System.**
