# Experimental Overview

This document focuses on how this project will go about exploring secure boot. 

## General Approach

The purpose of this study is to investigate how secure boot works, as such I have selected 4 different microcontrollers I want to examine. These microcontrollers are the Arduino Uno R3/Pro Micro, ESP32, and STM32-Nucleo, and the Raspberry Pi Pico. 

Because each of these microcontrollers have their own approach to secure boot (or in the case of the RPI Pico, no built-in option at all), I decided to first figure out a way to benchmark each of the microcontrollers to gain information about their baseline performance metrics with no extra security features enabled. Once I have baseline numbers, I intend to enable secure boot on the microcontrollers that have existing features and implement some sort of method for the RPI Pico. The benchmarks will then be run again and the performance will be compared. 

## Step-by-Step Process

1. Configure and execute benchmarks for microcontrollers
2. Enable or implement secure boot
3. Re-run benchmarks with secure boot enabled 
4. Compare performance metrics b/w secure boot enabled and disabled
5. (optional) Figure out ways to break and improve secure boot

## Benchmarks 

There are two main benchmarks that could work for gathering metrics. These are [Embench-iot](https://github.com/embench/embench-iot/tree/master) and [BenchIoT](https://github.com/embedded-sec/BenchIoT/tree/master). As of now, I intend to use embench-iot to run initial numbers, although I may use bench-iot in the future as it has more configuration options. 

The Embench-iot benchmark's github page mentions that it assumes the presence of no OS, minimal C library support and no particular outstream. It also requires that the device have more than 64kb of RAM, as all benchmarks should be run using 64kB. 

By default, there is a configuration file for a microcontroller with an ARM Cortex-M4 processor, STM32F4-Discovery Boards, generic AVR processors, and native among a few other options. As such, if I want to benchmark the ESP32, which uses an Extensa LX6 microprocessor, I will need to write my own configuration file. Reading into each of the config files, I believe its just intended to set compiler flags, but I need more time before I know for sure. I intend to post all custom configuration files in the github repo. 

Looking throught the ESP32 docs, it may be possible but it'll be difficult just because the ESP32 does not use gcc for compilation, it uses some wierd python framework.

- After looking through the python file, it turns out its just a wrapper to manage cmake and a few other things. I'm looking through [https://github.com/espressif/esp-idf/blob/master/tools/cmake/build.cmake]() to see if I can figure out what the gcc/g++ commands are.  
