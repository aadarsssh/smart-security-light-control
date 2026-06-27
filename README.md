# Smart Security & Light Control System

## 1. Aim
To design, simulate, and implement an IoT-based Smart Security and Light Control System that optimizes home energy usage and enhances automated security by activating light controls and cloud notifications only when specific environmental triggers are met simultaneously.

---

## 2. Problem Statement
Traditional automated lighting and security systems often operate independently, leading to inefficiencies. Continuous outdoor or indoor lighting causes unnecessary energy consumption during daylight hours. Conversely, motion-activated security alerts during high-traffic daytime hours trigger frequent false positives. There is a critical need for an intelligent system that accurately filters security threats and automates lighting *only* when human presence is detected in dark environments.

---

## 3. Scope of the Solution
This system provides a highly efficient, dual-purpose automation framework suited for:
*   **Residential Security:** Monitoring dark perimeters, driveways, backyards, and entryways during night hours.
*   **Smart Home Automation:** Activating convenient interior lighting only when someone enters a dark room, minimizing manual switch toggling.
*   **Energy Optimization:** Lowering corporate and residential electricity footprints by ensuring high-draw lights remain fully offline during daylight or vacant hours.
*   **Targeted Cloud Alerts:** Limiting push notifications or database logging strictly to suspicious events (motion in the dark), which saves network bandwidth and reduces alert fatigue.

---

## 4. Required Components

### Hardware & Simulation Architecture
*   **Microcontroller:** ESP32 (Selected for its low cost, high processing speed, and integrated Wi-Fi stack required for cloud connectivity).
*   **PIR Sensor (Passive Infrared):** To continuously monitor and detect human body movement via infrared heat signatures.
*   **LDR Sensor (Light Dependent Resistor):** To measure ambient light intensity and establish a daytime vs. nighttime threshold.
*   **Actuator (LED):** Serving as the hardware simulator for a smart bulb or automated security floodlight.
*   **Resistors:** 
    *   1x 10kΩ resistor (used in a voltage divider circuit with the LDR for accurate analog readings).
    *   1x 220Ω resistor (to limit current protecting the simulated LED).

### Software & Cloud Environment
*   **Simulation Platform:** Wokwi (Web-based embedded systems simulator).
*   **Development Framework:** Arduino IDE / C++ Core Framework.
*   **Cloud Endpoint:** HTTP/MQTT Protocol Network Layer (Simulated via Wokwi WiFi linking to an API endpoint such as ThingSpeak or Adafruit IO for real-time security logging).

---

## 5. Flowchart of the Code

Below is the logical execution path of the smart automation firmware. The system continuously polls the physical sensors, evaluates ambient conditions, and determines whether to trigger local hardware outputs and remote cloud webhooks.

```mermaid
graph TD
    A([Start / Power On]) --> B[Initialize Serial Monitor, Pins & Wi-Fi]
    B --> C[Read LDR Analog Value]
    C --> D[Read PIR Digital State]
    
    D --> E{Is Ambient Light < Threshold?\n(Is it Dark?)}
    
    E -- No (Daylight) --> F[Turn LED OFF]
    E -- Yes (Dark) --> G{Is Motion Detected?\n(PIR == HIGH)}
    
    G -- No --> F
    G -- Yes --> H[Turn LED ON]
    H --> I[Trigger sendCloudAlert Function]
    I --> J[Establish Connection to Cloud API]
    J --> K[Publish HTTP/MQTT Security Payload]
    K --> L[Enforce Cooldown Delay\n(5 Seconds)]
    
    F --> M[Wait 500ms Loop Delay]
    L --> M
    M --> C
