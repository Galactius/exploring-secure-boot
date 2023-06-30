# Microcontroller Notes

This document holds information regarding the various Microcontroller and dev-kits I encounted during my research. 

The difference between a microprocessor and a microcontroller is that a microprocessor is simply the IC, it requires external peripherals to be interacted with. A microcontroller is a board that includes a specific microprocessor and already has various peripherals integrated into it. For example, an Arduino UNO is a microcontroller that contains an ATMEL 3228PU microcontroller. 

Most microcontrollers can either be programmed using the Arduino Framework (through the Arduino IDE, also has its own Hardware Abstraction Language (HAL) which makes interacting with various sensors or actuators simpler than using registers) or using their own platform-specific IDE. Each platform has its own method of being programmed (whether its done through UART or an onboard USB-programmer), but all of the microcontrollers I looked at use USB. Generally, a microcontroller that does not have an onboard USB-programmer requires a FTDI programmer or some other programmer kit (such as STM's STLink) which enables the most functionality for debugging. A generic FTDI programmer uses the UART serial interface to reprogram the memory on the microcontroller. This is not typically reccomended if a USB interface or platform specific kit is available, as reprogramming through serial misses out on features such as breakpoints, debug modes, etc. 

## Arduino Framework Microcontrollers 

### Arduino UNO R3 Clone (RexQualis) 

While I do not own a genuine Arduino UNO R3, I do own a clone that serves essentially the same function and likely has the same pinout as the original Arduino Uno. My particular Uno microcontroller contains an ATMEGA 328P microprocesor, as well as various General-Purpose Input/Output pins (GPIO). 

### Inland Arduino Pro-Micro 

The Inland Arduino Pro-Micro is similar to the Arduino Leonardo/Micro but contains some extra peripherals, specifically access to an on-board USB programmer and debugger. The specific variant I have is a 5V model, a 3.3V model also exists. 

A common problem with this specific series of microcontroller is that in the Arduino IDE, it is very easy to accidentally flash a rogue program onto the microcontroller, as the Arduino IDE is not capable of detecting if the 5V or the 3.3V model is plugged in. This is significant because the 3.3V model runs at a much slower clock speed. If a rogue program is flashed onto this microcontroller, the only way to reprogram them is to enter bootloader mode by jumping the GND and RST pins, then flashing a valid program. However, there is only a 750ms window to flash the new program (8 seconds if the pin is jumped twice quickly). 

In my case, the time for the sketch to validate and upload was longer than 8 seconds, leading to my current Pro Micro being bricked. Also, these boards reportedly have issues with USB 2.0. 

My solution was to use a secondary Arduino Uno R3 I had as an ISP programmer, which essentially uses serial communications to burn the bootloader, which returns the Pro Micro to its original state before the rogue program was flashed. 

## STM32 Series

The STM32 series of microcontrollers vary significantly based on what specific board is being used. There exist four different STM32 families: Generic, Nucleo, Discovery, and Evaluation. Generic STM32 microcontrollers are often not made directly by STM but include a STM microprocessor. The Nucleo series is generally less expensive, but has a large variety of models ranging from low-cost to high-performance. THe Discovery and Evaluation development kits also have a large variety of models, but the discovery kits are closer to the nucleo-family in terms of price (but don't always include a USB programmer on board). The Evaluation dev-kits are signficantly more expensive, but also jam the most possible features and ports into the board itself. 

STM has also developed their own IDE: The STM32 Cube IDE, which makes developing for STM32-based development kits more user-friendly. The IDE features code-generation based on a GUI that allows the user to pre-define the specific board being used, as well as the pinMode for each pin being used. The IDE then auto-generates code based on the entered pin assignments, making pin definitions much more user-friendly, although they can always be manually defined in code. 

The STM32 HAL is more complex than the Arduino framework HAL, but it also offers significantly more performance than the arduino HAL. 

My suggestion is to find an inexpensive STM32 Nucleo-family board. Because this research project does not require specific hardware (as of now), any nucleo board should work. 

## ESP32 Series

The ESP32 series was developed by Espressif and features more IoT-friendly features, as well as a dual core CPU through their ESP32-WROOT microprocesor. Most ESP32 dev-kits contain a WiFi and Bluetooth chip on-board, allowing developers to implement IoT-related features. EspressIf does not have its own IDE, but it does provide plugins for VS Code and Eclipse, as well as a CLI. Many hobbyist developers reccomend using PlatformIO (also available as a VS Code extension) to develop programs for ESP32s. 

I personally own a Inland ESP32 dev-kit as well as a Makerfun (Heltec Automation) ESP32-WiFI kit with an onboard OLED screen. Both differ slightly from the original EspressIf design, but I have the correct documentation for both. Both of these specific dev-kits also include arduino framework support. The problem with both of these boards (and essentially any board) is the lack of standard pinouts for each board. In the case of the Inland ESP32, the pinout that is provided does not name the pinouts according to their name written on the PCB. Also, in order to flash a program on the Inland ESP32, I had to hold the boot button on the device before I could flash a program to it. 

The ESP-32 Module on the Inland microcontroller is the ESP32-WROOM-32D, there is also a -32U module that differs slightly in that the -32U requires an external antenna, whereas the -32D has an on-board antenna built-into the PCB.

The CPU is a dual-core Extensa LX6 MCU, there are two CPU cores that can individually controlled, the CPI clock is also adjustible between 80-240Mhz. The OS chosen for the ESP32 is freeRTOS with LwIP and it has a TLS 1.2 with hardware accelearation built in. The CPU contains 448Kb of ROM (for booting and core functions), 520Kb of on-chip SRAM, 8Kb SRAM for RTC FAST and SLOW memory as well as the 1kb eFuse.

The ESP32 Module on the Makerfun/Heltec Automation ESP32 microcontroller is based on the ESP32-S3, which has the advanced features. 384Kb ROM, 512 Kb SRAM.

Note that the ESP32 DOES NOT use an ARM processor, it uses an Extensa LX6 rather than the RP2040's ARM Cortex-M0+.