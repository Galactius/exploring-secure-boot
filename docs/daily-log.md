# Daily Research Journal

## Day 1 - 5/30/2023 

Today is my first working day on this project, I started working at 10:30AM, then stopped at 3:30PM. 

I started today by reviewing the project requirements as described in the schedule provided by Dr. Nwafor. I then setup a Trello board with this information and shared it to Dr. Nwafor. I also started picking out which papers I wanted to start reading. I chose 5 papers and downloaded them to my 2023 research onedrive folder and saved their citation links in Semantic Scholar. Next, I setup my development environment by downloading VS Code and Obsidian and configuring them as needed. I ended the day by setting up a github repo with a documentation site hosted through readthedocs. However, the deployment of the docs site had some issues (a deprecated dependency issue I've seen and fixed before). After a bit more time, I was able to fix the issues with the docs site, which is now available at exploring-secure-boot.readthedocs.io. Remaining work today will be spent continuing to configure the docs site and reading papers. 

## Day 2 - 5/31/2023

Today I started working at 11:00AM and stopped at 4:00PM. 

I started the day by finishing up fixing the issues with the documentation site, which took some time as I had to reset the site on the host (which is readthedocs for this project). After fixing the website and ensuring that my VS Code configuration was working properly, I continued to read papers starting with "A Survey of Secure Boot Schemes for Embedded Devices", as well as some online articles describing how to create a custom ESP32 breakout board. I'm not sure that we will end up needing to create a custom PCB unless we decide to implement some hardware-based secure boot method (which I would need to do much more research on as I have never created a custom PCB). I picked an ESP32 to start looking into just because I have my personal ESP32 microcontroller that I can reference if needed later. My personal notes for each paper I read are not pushed to the github as of now. 

## Day 3 - 6/1/2023

Today I started working at 9:30 AM and stopped at 4:30PM. 

I spent most of today reading papers and investigating various microcontrollers. At first I was trying to see what microcontrollers were available at a local elecronics store, which led to product pages for individual microcontroller families which had information about their existing secure boot methods. As of right now, I have information regarding the secure boot method on an ESP32(https://blog.espressif.com/understanding-esp32s-security-features-14483e465724) and an STM32(https://www.st.com/en/embedded-software/x-cube-sbsfu.html#overview). Both also had provided further documentation for how secure boot is implemented on both devices. Based on my readings so far, I also started updating the background info page which will remain unpublished until I have more information written. 

## Day 4 - 6/2/23 

Today I started working at 11:30AM and took a break at 1:30PM. I then started working again from 4:30PM to 7:30PM. 

After my meeting with my advisor, I started to read through most of the resources I compiled throughout the week and continued writing notes as well as my background page. I was also told to begin a literature review of embedded systems which I intend to integrate into my background page. 

## Day 5 - 6/3/23 

Today I started work at 1:00PM and stopped at 8:00PM. I spent most of the day reading whitepapers, and writing down my notes in my private notes. My plan for right now is to focus on reading through the whitepapers and gaining a good enough understanding to write the background without relying fully on the resources, while still referencing them. As of 8PM, I have completed my notes for 3 different whitepapers and 2 formal papers. 

## Day 6 - 6/4/23 

Today I started working at 1:00PM and stopped at 8:00PM. 

I continued reading through papers, writing down my notes, and paraphrasing information into the background page. I also purhased some new microcontrollers for personal use that I intend to prepare today and potentially use later for research. I purchased an Inland ESP32 and a Raspberry Pi Pico W. After I finish preparing and testing both MCUs, I am curious to look into the bootloader and whether secure boot is on by default or not. I also started reviewing C and C++ programming. 

## Day 7 - 6/5/23 

Today I started working at 7:30AM and stopped at 2:30PM. 

I started the day by watching videos on how to select and program STM32 and ESP32 boards. I already own an ESP32 board, but I believe an STM32 board would be interesting to consider for this project, although I have no actual experience with a STM-branded MCU. I also contiuned reviewing C/C++ programming for embedded specifically and continued reading papers and whitepapers. I also started setting up some of the microcontrollers I could use for this project


## Day 8 - 6/6/23 

Today I started working at 8:00AM and stopped at 1:00 to break for lunch. I continued my work from 3:30 to 5:30. 

Most of my progress today went towards testing my microcontrollers by running a blink test program on each of the microcontrollers I had, as well as continuing the literature review. The reason that verifying my microcontrollers took so long is because I was using the Arduino IDE (which is not standard for each of these boards), and each of the microcontrollers has some quirks that are not always clear. For the literature review, I focused on reading through the STM, ESP, and RPI documentation for secure boot, as this information is pertienent to the current implementation for these devices. 

## Day 9 - 6/7/23 

Today I started working at 11:00AM and took a break at 1:00PM for lunch. I continued working from 2:00PM to 5:00PM.  

Since I was able to test the rest of my microcontrollers yesterday, I focused on the literature review for today. 

## Day 10 - 6/8/23 

Today I started working at 11:00 AM and stopped at 1:00PM for lunch. I then continued from 3:30 to 5:30PM. 

Today I am wrapping up my readings and plan on starting the writing this evening and continuing it into tomorrow/this weekend. 

## Day 11 - 6/9/23 

Today I started working at 6:30AM and stopped working at 3:30PM. 

My plan for today is the same as yesterday with a focus on completing the literature review. 

## Day 12 - 6/12/23

Today I started working at 11:00 AM and stopped working at 4:00PM. I also worked for 4 hours on sunday. 

Today I plan to start wrapping up the literature review and selecting the microcontrollers I want to use for this project, I also plan on spending some time practicing my C/C++. 

## Day 13 - 6/13/23 

Today I started working at 11:00AM and stopped working at 5:00PM. I just continued my work from yesterday. 

## Day 14 - 6/14/23 

Today I started work at 9:30 AM and stopped working at 11:15AM. 

I am selecting the 3 microcontrollers I want to test, documenting my reasoning, and finishing the literature review. 

## Day 15 - 6/16/23 

Today I started working at 6:00AM, then I took a break at 8:45. I continued working from 7:30PM to 11:30. 

I continued the literature review, had my weekly meeting with my advisor. As of the meeting, I have decided to focus on the Raspberry Pi Pico and the ESP32 for experimentation. After I complete the literature review, I intend to complete a deeper dive on the RPI Pico and the ESP32's security features. 

## Day 16 - 6/17/23

Today I started working at 11:00AM and stopped working at 4:30PM. I then returned from 6:00PM to 8:00PM. I also worked two more hours from 10:00PM to midnight. 

I spent most of today continuing the literature review and reviewing exercises on C/C++ as well as the arduino framework. I also spent some time investigating the external TPM module for RPI's. 

## Day 17 - 6/18/23 

Today I started working at 12:00PM and stopped at 2:00PM, then continued working from 6:00PM to 10:00PM. 

My work today was a continuation of what I worked on yesterday. 

## Day 18 - 6/19/23 

Today I started working at 10:30AM and stopped at ... 

Today I intend to complete the literature review, continue exercises using the arduino framework, and investigating the external TPM module. 

## Day 19 - 6/20/23 

Today I started working at 10:00AM and stopped at 1:00PM, then contined working from 8:30 to 11:30PM. 

Today I focused on learning more about the External TPM module and watching conference lectures that mentioned an external TPM. 

## Day 20 - 6/21/23 

Today I started working at 10:30 and stopped at 2:00PM. 

Today I focused on looking for potential benchmarks I could run on the RP2040 and the ESP32, as well as contuiniing my write-up of the external TPM module and the literature review.

## Day 21 - 6/22/23 

Today I started working at 10:30AM and stopped at 1:00PM, then continued from 4:00PM to 8:30PM.  

I found a benchmark I could use: embench-iot, but it requires a certain amount of RAM and only has configuration for ARM and AVR processors. As such, I may have to look into acquiring an STM board that already has a config for the benchmark. My plan for today is to look for a potential STM board to use for experimentation, then start reading through the configuration files to setup the experiments. 

## Day 22 - 6/23/2023 

Today I started working at 11:00AM and stopped at 2:30PM, I then continued from 7:00 to 10:00PM. 

After meeting with my advisor today, I spent the day finishing up the background and setting up the outline for running experiments on the docs site. I then focused on researching things we discussed during our meeting.

## Day 23 - 6/26/2023 

Today I started working at 10:30AM and stopped at 2:00PM, then continued from 4:00PM to 7:30PM. 

Today I found which STM board to order as well as did a bit of research on some of the security features STM offers, and started trying to configure one instance of the embench-iot benchmark. 

## Day 24 - 6/28/2023 

Today I started working at 11:00AM and stopped working at 2:30PM, then continued from 8:00PM to 11:30PM. 

I decided to rewrite the background based on updated information after having re-read some of the documents I previously had. 

## Day 25 - 6/29/2023 

Today I started working at 12:00PM and stopped working at 4:00PM. 

I completed the rewrite of the background today and started writing down my plan for running experiments. The rest of my time today will be dedicated to trying to get embench-iot configured. 