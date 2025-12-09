# 🔒 Secured_Door_Lock_System_Using_ESP-NOW

A secure, two-part door access control system using two **ESP8266 NodeMCU** boards. The system provides password authentication and physical access control for smart home and IoT security applications.


---

## ✨ Features

* **Secure Access:** Password authentication ensures the lock is opened only when the correct code is entered.
* **Dual NodeMCU Architecture:** One ESP8266 acts as the **Sender** (keypad interface), and the other acts as the **Receiver**.
* **Visual Feedback:** A **16x2 LCD display** shows system status, including "Enter Code," "Permission Granted," or "Wrong Identification".
* **Audible Alarm:** An active buzzer alarm/siren activates for incorrect password attempts and can be silenced using a **reset button**.
* **Automatic Lock:** The solenoid lock is only unlocked for a short duration (5 seconds) after successful entry.
* **Wireless Communication:** Uses **ESP-NOW** for fast, local communicati between the Keypad (Sender) and Lock (Receiver).

---

## ⚙️ Hardware & Software Requirements

### Hardware Requirements

* 2 x ESP8266 NodeMCU modules
* 4x4 Keypad
* 16x2 LCD with I²C interface
* 12 V Solenoid Lock
* 4-channel Relay Module (1 channel used)
* 2 x Active Buzzers (one for each board)
* LED with Resistor
* Push Button (Reset)
* Power Supplies (12 V for solenoid, 5 V for boards/relay/LCD)

### Software Requirements

* Arduino IDE
* **Keypad** library
* **LiquidCrystal\_I2C** library
* **ESP8266WiFi** library
* **WiFiUDP** library

---

## 💡 Working Principle

The system operates based on two wirelessly communicating microcontrollers:

1.  **Sender NodeMCU (Keypad Interface):**
    * Reads input from the **4x4 keypad**.
    * An audible buzzer provides feedback for each keypress.
    * The entered password buffer is cleared by pressing the ***\* (asterisk)*** key.
    * The complete password is transmitted to the Receiver NodeMCU via **UDP** only when the **`#`** key is pressed.

2.  **Receiver NodeMCU (Lock Controller):**
    * Continuously monitors for an incoming UDP packet containing the password.
    * The received password is compared against a pre-stored correct password.
    * **On Success:** The relay activates, the solenoid lock unlocks for 5 seconds, and the LCD displays "Permission Granted - Welcome Home". The buzzer and LED give a positive confirmation signal.
    * **On Failure:** The LCD displays "Wrong Identification," and the buzzer/LED activate a **siren alarm**.
    * The siren alarm can be deactivated, and the system returned to the idle state using the **Reset Button**.

---

## 📚 Literature Review

Traditional security systems often rely on wired connections, which can be vulnerable to physical tampering. This project leverages the low-cost and powerful **ESP8266** microcontroller's built-in **Wi-Fi capabilities** to create a robust wireless communication link between the user input (keypad) and the physical control mechanism (lock/relay). The use of two separate boards enhances security and modularity. The implementation of **UDP** is a simple, lightweight protocol suitable for rapid, non-critical data transmission, like a password string, in a local network environment.

---

## 🔗 Circuit Connections

The detailed connections are essential for proper operation.

### 🔑 Keypad/Transmitter ESP8266 (Sender)

| Component | Component Pin | ESP8266 Pin | Type |
| :--- | :--- | :--- | :--- |
| **Keypad** | Row 1 (R1) | D0 | Digital Out |
| **Keypad** | Row 2 (R2) | D1 | Digital Out |
| **Keypad** | Row 3 (R3) | D2 | Digital Out |
| **Keypad** | Row 4 (R4) | D3 | Digital Out |
| **Keypad** | Column 1 (C1) | D5 | Digital In |
| **Keypad** | Column 2 (C2) | D6 | Digital In |
| **Keypad** | Column 3 (C3) | D7 | Digital In |
| **Keypad** | Column 4 (C4) | D8 | Digital In |
| **Active Buzzer** | Signal/Control | **D4** | Digital Out |
| **Keypad, Buzzer** | Power/GND | VCC/GND | Power/Ground |

### 🔒 Lock/Receiver ESP8266 (Receiver)

| Component | Component Pin | ESP8266 Pin | Type |
| :--- | :--- | :--- | :--- |
| **I2C LCD** | SDA | D2 | I2C Data |
| **I2C LCD** | SCL | D1 | I2C Clock |
| **Relay Module** | Signal/IN | **D6** | Digital Out |
| **Status LED** | Signal (Anode) | **D7** | Digital Out |
| **Active Buzzer** | Signal/Control | **D4** | Digital Out |
| **Reset Push Button** | Signal | **D5** | Digital In (PULLUP) |
| **I2C LCD, Relay, Buzzer, Button** | Power/GND | VCC/GND | Power/Ground |

## 🛠️ Setup and Installation

### 1. Libraries

Install the following libraries in your Arduino IDE:
* **ESP8266WiFi** (Built-in)
* **espnow** (Part of ESP8266 core)
* **Wire** (Built-in)
* **LiquidCrystal\_I2C**
* **Keypad**

### 2. MAC Address Configuration

The **Sender** must know the **Receiver's** MAC address:
1.  Upload the **Receiver Code** and check the Serial Monitor. It prints the receiver MAC address.
2.  Copy this MAC address.
3.  In `Sender_Code.ino`, replace the placeholder in `receiverAddress[]` with the copied MAC address.

'''cpp
// Replace with your receiver MAC address
uint8_t receiverAddress[] = {0x84, 0x0D, 0x8E, 0x99, 0xE8, 0xA2};'''

---

## 🎯 Applications

This system is an excellent prototype for several low-cost IoT and security applications:

* **Smart Home Security:** Providing keyless entry to homes or restricted rooms.
* **Locker/Cabinet Security:** Integrating the system into cabinets or small safes for authenticated access.
* **Proof-of-Concept for IoT:** Demonstrating inter-device communication and control using the ESP8266 platform.
* **Laboratory/Server Room Access:** Controlling access to sensitive areas with simple password authentication.
