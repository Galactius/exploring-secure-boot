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