---
mode: agent
---
1. Project Goal:
The primary goal is to create a speed limiter for my electric moped, controlled by an ESP32-C3. The firmware must be built using the ESPHome framework.

The core functionalities are:

The speed limiter is toggled ON or OFF by scanning an authorized 13.56MHz RFID/NFC tag.

The list of authorized RFID tags must be manageable via a web interface hosted on the ESP32 itself, allowing for adding and removing tags.

2. Hardware Components:

Microcontroller: ESP32-C3 Super Mini

RFID Reader: MFRC522 SPI Module

Switching Circuit: BC337 NPN Transistor with a 1kΩ base resistor.

Power Supply: A DC-DC step-down converter providing 5V to the ESP32.

3. Pinout / Wiring (ESP32-C3 Super Mini):
The project must use the following "safe" pinout, which avoids the ESP32-C3's strapping pins.

Function	ESP32-C3 Pin
Limiter Control (to 1kΩ resistor)	GPIO7
RC522 - CS / SDA (Chip Select)	GPIO5
RC522 - SCK (Clock)	GPIO4
RC522 - MOSI (Data Out)	GPIO3
RC522 - MISO (Data In)	GPIO10
RC522 - RST (Reset)	GPIO6
RC522 - 3.3V Power	3.3V
RC522 - GND	GND

Exporteren naar Spreadsheets
4. Required Functionality & ESPHome YAML Structure:
I need you to generate a complete ESPHome YAML configuration file from scratch that accomplishes the following:

Wi-Fi: The device must have both standard client mode (for connecting to a home network) and an Access Point (AP) mode to act as its own Wi-Fi hotspot for on-the-go configuration.

Web Server: The web_server component must be enabled.

RFID Reader: The official rc522_spi component must be configured with the pinout above.

Limiter Switch: An output and a template switch must be configured to control the limiter circuit on the specified GPIO pin.

Tag Management Logic:

A global variable (globals) must be used to store a list of authorized RFID UIDs. This list should be persistent across reboots (restore_value: yes).

When an authorized tag is scanned (on_tag), the limiter switch should be toggled.

When an unauthorized tag is scanned, its UID should be published to a text_sensor named "Last Scanned Tag" so it is visible on the web interface.

Web Interface Functionality:

The ESPHome web interface should display the list of all currently authorized UIDs. This should be done by publishing the list to a text_sensor named "Authorized Tags List".

Two custom services must be created for tag management:

add_tag(uid): This service accepts a UID string and adds it to the global list of authorized UIDs.

delete_tag(uid): This service accepts a UID string and removes it from the global list.

After a tag is added or deleted via a service call, the "Authorized Tags List" text_sensor must be updated to reflect the change.

5. Task:
Please generate the complete moped_controller.yaml file that implements all of the above requirements. Ensure the code is clean, well-commented, and follows ESPHome best practices.