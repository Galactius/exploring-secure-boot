# Notes from resources: 

This document describes notes I have written for the various resources and documents I have read. These notes will be used for future reference and have been mostly paraphrased to make it easier for me to find references or specific information from the various sources I have read. I primarily focuesed on reading whitepaper, although some formal papers also have notes.

## "A survey of Secure Boot Schemes for Embedded Devices" (Embedded Secure Boot Survey Paper)

- Secure boot is "a process of measuring the integrity and authenticity of the operating system kernel and application software running on the device at the booting time" 
- Secure boot guarantees "that the embedded devices boot up with an untampered and authorized software provided by a legitimate vendor"
- Existing models of Secure Boot already exist but may not be appropriate for embedded systems as they are resource-constrained. 
- Three different embedded secure boot schemes: software-based, hardware-based, and hybrid. There are by far many more hardware-based than software or hybrid mentioned in this study.  
- Game me a few other papers to read... 


## UEFI Secure Boot In Modern Computer Security Solutions (UEFI Secure Boot Whitepaper)

- https://uefi.org/sites/default/files/resources/UEFI_Secure_Boot_in_Modern_Computer_Security_Solutions_2013.pdf
- Interesting history about BIOS: The Basic Input/Output system was written by IBM in assembly for computers using 16-bit 8086/8088 Intel processors in the 1970s, that firmware implementation was expanded for future use. In the 1990s, Intel developed the Extensible Firmware Interface (EFI) as an alternative to BIOS for their 64-bit Itanium processors and released the standard specificaiton in 2002, which was then transferred to the United EFI (UEFI) forum. In November 2010, the UEFI forum developed a specificatio that drastically enhanced security for booting OSes. 
- Attacks that target the time between firmware and operating system code has arisen since the 80's. This is because anti-malware and anti-virus software has becoming increasingly effective. Rootkits or Bootkits that target systems before the operating system boots would circumvent all of these countermeasures. In order to protect from these forms of attacks, UEFI and secure boot technologies were developed. 
- Rootkits and Bootkits are particularly difficult to detect and remove, as malware hidden in firmware cannot be easily detected by an OS unless it has tools to scan firmware. Rootkits and bootkits are also dangerous in that firmware provides the malware hardware-level access, which allows it to make use of Virtual Machine technology to mask itself or embed itself into the System ROM, which makes the malware impervious to OS reinstallation and hard-drive replacement. 
- Firmware rootkits work by patching or replacing the default firmware image, while bootkits inject malware at the point between firmware and OS code execution. Various kinds of attacks, such as the evil maid attack (where someone installs a bootkit on a public, unattended PC which can then capture user credentials), or bootloader hijacks exist. 
- Some existing firmware protections include the Trusted Computing Group's Trusted Platform Module (TPM) which takes measurements of all phases of the booting process and allows for an OS to detect modified boot code, although it does not inherently prevent it. 
- The US Govt's NIST guidelines 800-147, 800-147B, and 800-155 provide guidelines for BIOS protections but do not solve bootkit attack issues. 
- Keys and signatures are used to verify code before its executed. The Platform Key is set by a manufacturer in the factory, while it can be replaced its primary use is to protect the Key Exchange Key from uncontrolled modification. THe KEK protects the signature database from unauthorized operations, and operations cannot occur without the private portion of the key. The database contains authorized and forbidden code which can be accessed based on whether the accessor is trusted. As a computer boots, a code section is loaded (but not yet executed), its signature is confirmed using the signature database, that code section executes if and only if that signature matches the same one in the signature database. 
- UEFI secure boot is optional, can be disabled, and can also be reinforced using a TPM module, or other security features. The point of UEFI is that the standards are open, not every part of the standard needs to be active, although it is possible to secure devices more. 

## Cisco: Secure Boot: An Effective, Low-Risk Alternative to Commercial Solutions (Short Secure Boot Whitepaper)

- https://www.cisco.com/c/dam/en_us/about/doing_business/trust-center/docs/secure-boot-white-paper-20151009-edcs.pdf
- Secure boot is a technology that prevents start-up code from being modified and protect against malware, logic bombs, and nefarious instructions. It works by "anchoring trust in an element of the system that is rigorously controlled", which is then used to "begin the authentication chain and validate the integrity of the rest of the system". The authentication chain is layered, the previously loaded code must be authenticated before the next layer can be reached. 
- The most important way to mitigate threats of the persistent execution of unauthorized code is to ensure the system boots only authenticated code. This begins with the root of trust. The root helps recover compromised systems and prevent injection scenarios. 
- One major common secure boot technique used in CPUs is to "measure" or hash software as it boots. The measurement itself it taken by a protected environment (likely a trusted platform), where the result is stored in "tamper-resistant" storage. An external system then verifies if the hash is authenticated or not and acts based on the status of the hash. This is called the "measured boot" approach. 
- Another common method is to use an "immutable anchor" (likely a Trusted Platform Module) inside the CPU which validates and sometimes decrypts software that will be executed on the CPU, that way the system can validate code before it is run. 
- A problem with secure boot is that most implementations are CPU-specific. With measured boot, the system that handles failed or unauthorized code has also presented challenges in embedded systems. Also, the key or root of trust used can become a risk if the key is compromised or mishandled, especially in systems that only use one root of trust. At the time of writing, no system handled key revocation. Also, some CPUs had models with and without secure boot, which enabled attackers to swap CPUs in certain systems. 
- An alternative system-level solution (so a solution that requires changes in the design phase, not during or after manufacturing) is to add an FPGA or an ASIC unit that handles the secure boot logic and has control over the CPU's start sequence. This is advantageous in that this means a design could be CPU-agnostic, the logic unit could be an off-the-shelf component, and it could be much more flexible for security improvements as long as an ASIC is not used. 

## Linux-Kernel Trusted Execution Environment Documentation

- https://docs.kernel.org/staging/tee.html
- Github for TEE: https://github.com/OP-TEE/optee_os
- Docs for TEE code: https://optee.readthedocs.io/en/latest/
- A Trusted Execution Environment is a generic term for a secure environment, whether this be done through some sort of special hardware mode (such as the case for ARM TrustZone) or an entirely separate processor. 
- A TEE driver communicates with the TEE, where the driver handles the driver registration, manages shared Linux and TEE memory and provides a generic API. The driver should not really see communications between the client and the TEE, it should simply receive and forward TEE and send back the TEE results of a client's query. 
- The directory: `include/uapi/linux/tee.h` defines a generic interface to a TEE, the client can connect to the driver by opening /dev/tee[0-9]* or /dev/teepriv[0-9]*. A normal client opens the former and a supplicant (a helper process for the TEE to access Linux resources) opens the latter. 
- There are two special implementations of TEE: OP-TEE which seems to be an open source implementation of TEE intended for insecure Linux kernels exclusively for ARM processors and AMD-TEE, which is exclusive to AMD CPUs. 
- More info on OP-TEE: https://wiki.st.com/stm32mpu/wiki/OP-TEE_overview
- In both OP-TEE and AMD-TEE, the user space or client, kernel, and secure world (or AMD Secure Processor for AMD-TEE), are separate but connected using interfaces. The TEE Client API interacts between the client and the kernel, while the a generic TEE API interacts between the Client API and the TEE subsystem within the kernel. There will then be a OP-TEE or AMD-TEE driver that interacts between the TEE subsystem with the kernel and the secure world. The driver then communicates with the OP-TEE or AMD-TEE trusted OS through some sort of messaging protocol. The final layer is some sort of internal secure API which connects the trusted OS to the Trusted Application. 

## OP-TEE Overview (from STM)

- https://wiki.st.com/stm32mpu/wiki/OP-TEE_overview
- Link to TEE Group: https://www.trustedfirmware.org/projects/op-tee/
- OP-TEE is essentially a way to implement a trusted execution environment using Arm TrustZone. 
- When using OP-TEE there exist two different "worlds" a secure section of memory which contains the OP-TEE core, the Core API library and the trusted applications, then an insecure world which contains the Linux kernel with an OP-TEE driver, user applications, a TEE supplicant and a TEE Client API Library. 
- The OP-TEE core in the secure memory loads signed, trusted applications from the Linux OS file system or from an embedded image in the OP-TEE core. 
- The OP-TEE Linux driver has been a part of linux since 4.12. 
- The TEE client API is partd userland library and part Linux Kernel OP-TEE driver, and allows userland clients to invoke trusted applications and the OP-TEE core services in the insecure world. 
- Because OP-TEE is secure firmware, it must be booted prior to the non-secure world on Arm Cortex A-cores. The secure bootloader must load the OP-TEE images in memory and init, them prior to executing the first non-secure image. 
- There are three sequences in which OP-TEE can be invoked. Sequence A is when a insecure application calls the TEE client API library which then invokes the Linux Kernel OP-TEE driver. This then invokes the secure world by reaching the OP-TEE core which transfer the request to the trusted applcation. Sequence B is when the OP-TEE core invokes the OP-TEE driver which notifies the TEE supplicant daemon for a request. Sequence C is when a trusted application invokes the OP-TEE core service. This is when the trusted application calls the TEE internal Core API library which can be used to access the OP-TEE core. 

## Arm Trustzone

### Contextualization
- The aim of the ARM Trustzone technology is to enable a device to benefit from a feature-rich open operating environment and a robust security solution. ARM Trustzone works bey integrating system-wide protection measures into the ARM processor, bus fabric, and system peripheral IP. This requires integration from hardware and software. 
- The classic security solution for embedded applications is the inclusion of a dedicated hardware security module, or trusted element, that is outside the main SoC (p. 27)
- The external hardware security module is advantageous due to the high levels of tamper resistance and physical security as well as the fact that most solutions now have been certified and approved through rigourous evaluation schemes. However, hardware modules are more expensive to develop for an SoC and require much more work at the design phase of a project for true integration. 
- The internal security module (one within an SoC) comes in two main forms: a hardware block which manages cryptographic keys and operations or a general purpose processing engine placed alongside the main processor which uses custom hardware logic to prevent unauthorized access to sensitive resources. Internal security modules significantly reduce the cost of development and increase performance over common external security module solutions. ARM Trustzone is similar to the general purpose processing engine. A disadvantage of using a cryptographic hardware block is that the keys stored inside are useless if they are exposed, which can happen when operations outside of the hardware block are done using the keys. The issue with the secondary general purpose processor is the fact that this must be included at the design phase of a project, and the processor itself takes up much silicon space apart from the resource overhead required. 
- An important consideration for security is the acessability of debug interfaces, such as JTAG. If these interfaces are left enabled or can be easily accessed, they can become an avenue from which an attacker can exploit the device from. 
- Another method is to use software virtualization, where a hypervisor runs in a priveledged mode of a processor and runs multiple independent software platforms using the Memory Management Unit, each platform running inside a VM. This is called paravirtualization, whereas some embedded sytems have a special processor mode for hypervisors. 
- Any processor with an MMU can be used to implement some sort of virtualization solution, the problem is how implmentation works for applications that seek to use secure memory. Because the hypervisor is isolated, external devices such as DMA engines or GPUs must be managed by the hypervisor, which often leads to significant performance decrease.

### Implementation 
- ARM's approach to trusted computing is to develop a trusted platform that extends security throughout the system design rather than just having a secure portion of memory. 
- The extended bus design has an added control signal for each of the read and write channels on the main system bus. Because of this AMBA3 AXI bus modification, peripheral devices can be secured and a bridge will make sure that only the correct resources are accessed. 
- In terms of processor architecture, each physical core on a processor will have two virtual cores, a secure and an insecure core. A mechanism to switch between then called monitor mode is also used. The non-secure virtual processor can only access non-secure system resources while the secure processor can see all resources. This monitor mode is also used for time-slice-based context switching. 

## External TPM Module

- My advisor made me aware of an external TPM module that could potentially be used for this project. 
- Link to first product page: https://www.amazon.com/GeeekPi-Raspberry-Infineon-OptigaTM-Compatible/dp/B09G2BZQT5?source=ps-sl-shoppingads-lpcontext&ref_=fplfs&psc=1&smid=AOP0CH6UTUPHT
- Based on the product page: this is the chip used on the device: https://www.infineon.com/cms/en/product/security-smart-card-solutions/optiga-embedded-security-solutions/#
- Github Page with more info: https://github.com/Infineon/optiga-tpm
- Github Page with software for TPM: https://github.com/Infineon/eltt2
- TPM & IoT Conference with same TPM: https://www.youtube.com/watch?v=S6HWK8PF5MU, note this is 3 years old! 
- Based on the second github page, the Infineon TPM is only compatible with little-endian hardware and software capable of running Linux and hosting a TPM. Based on the datasheets for the ESP32 and the RP2040, both are little-endian, but neither may be capable of running linux. I may have to look into the driver, it may also be possible to re-write the driver to work with the ESP32 or the RP2040. 
- The way that the TPMs is that on RPI boot, the GPU Broadcom Firmware will look for a file called bootcode.bin, within this file/folder a tpm-... file is included with Raspbian that contains the code needed to setup the TPM. From there start.elf will use config.txt (from which SPI and dtoverlay must be enabled), which then starts the kernel image. 
- The Linux kernal already contains the tpm driver! 

# Embench-IoT (Embedded Benchmark Suite)

- https://github.com/embench/embench-iot/tree/master
- Potential Alternative: BenchIoT. 
- Open source set of benchmarks for "deeply" embedded systems. Requirements are to have 64kb of RAM and ROM. Not entirely sure if the RP2040 will work, although the datasheet describes having 256kb of SRAM and 16kb of ROM. The readme in the root of the gitub only mentions the RAM requirements, nothing about the ROM. A quick google search for the ESP32 specs suggests that it meets both requirements.
- Now that I think about it, its possible that implementing secure boot could take up enough memory to not meet the requirement. 



## ESP32 Security

### Understanding ESP32's Security Features

- https://blog.espressif.com/understanding-esp32s-security-features-14483e465724
- Link to ESP32 Secure Boot Docs: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v1.html (different from latest version of Secure Boot). 
- Link to ESP32-S2 SoC Security Features: https://blog.espressif.com/esp32-s2-security-improvements-5e5453f98590
- Note, there are new security features in the latest ESP32-S2 SoC, this article focuses on the ESP32v3 security features. 
- Its possible that secure boot requires the ESP-IDF plugin in order to be configured properly. 
- Flash encryption and secure boot features protect devices from unwanted access to the SPI flash memory that comes on-board a device from the factory, but are opt-in, so they have to be implemented by either the manufacturer of a product using ESP32's or an end-user.  
- The eFUSE is a one-time programmable memory containing four blocks with 256-bits in each block. Block 0 is reserved, block 1 is used for Flash Encryption, block 2 is for secure boot, and block 3 is reserved. The eFUSE will store keys used for flash encryption and secure boot which cannot be changed, it can also be configured in such a way that only the ESP32 hardware (not the software) can read the keys. The eFUSE seems to be what contains the **root-of-trust** for the ESP32 platform. 
- The eFUSE block 2 key is then used to establish trust between the eFUSE and the software bootloader (keep in mind there is a separate section of ROM bootloader which contains the eFUSE). Once the software bootloader is verified by the eFUSE, the software bootloader is used to verify the application firmware flashed by a user. 
- Flow looks like: Reset -> BootROM & eFUSE -> Software Bootloader -> Application Firmware
- The software bootloader is comprised of the bootloader image, an RSA signature, and an RSA public key. On reset, the eFUSE validates the RSA public key which then validates the RSA signature and the bootloader. Note: the RSA private key used to generate the signature is kept with the manufacturer. 
- Once the software bootloader is verified, the BootROM transfers execution control to the software bootloader which then validates the application firmware. The validation process for the application firmware is essentially the same as for the software bootloader. 
- ESP32's previously did not support RSA but some other encryption algorithm, or none at all. 
- ESP32's also have two extra security features: Flash Encryption and JTAG/UART Boot Disable. Flash encryption is essentially a method of encrypting application firmware so that an end-user cannot see a manufacturer's firmware. Flash encryption makes use of the eFUSE's first block. For the JTAG/UART disable, the eFUSE has a one-time reprogrammable bit field that can permanently disable JTAG debugging and UART booting. 


### ESP32-S2 - Security Features

- https://blog.espressif.com/esp32-s2-security-improvements-5e5453f98590
- The ESP32-S2 is a newer iteration of the ESP32 SoC, but it includes new hardware and security features. 
- The flow for Secure Boot v2 is the same as in v1, but an AES Symmetric key and SHA secure digest algorithm are used. The ESP32-S2 also has an improved RSA accelerator, similarly, improvement to the secure boot algorithm decrease the signature verification time to under 100ms. 
- In terms of flash encryption, the ESP32-S2 implementation is essentially the same but a AES256-XTS encryption scheme is used. 
- An added hardware module on the ESP32-S2 is the Digital Signature Peripheral. The hardware block allows applications to perform RSA digital signature operations without allowing the application access the RSA private key, which is important for authentication schemes that involve using the cloud. 
- The ESP32-S2 also has 4096 bits of eFUSE memory compared to the ESP32's 1024 bits. This means that the application firmware has more memory for generating per-device unique identifiers. 
- Lastly, the ESP32-S2 also fixes a hardware bug that allowed maliscious users to bypass secure boot and read encryption keys by using physical voltage glitching. 


## STM32 Secure Boot

- OP-TEE is basically a method to execute secure or trusted functions in an isolated environment within a Linux-based OS. Its a open source project that creates a complete Trusted Execution Environment using ARM's Trustzone technology. 

### X-CUBE-SBSFU Overview

- Secure Boot Overview: https://www.st.com/en/embedded-software/x-cube-sbsfu.html#overview
- The feature is called X-CUBE-SBSFU, and enables secure boot functionality as well as secure firmware update which allows for secure update of firmware that prevents unauthorized updates or access to confidential on-device data. 
- Secure Boot works by running checks on static protections, run-time protections, and verifying user application code on reset. The code for secure boot is immutable. 
- Secure Firmware Update seems to be a proprietary method of uploading secure application firmware using UART and a protocol called Ymodem, the protocol then verifies the integrity of the uploaded application code before installing it. 
- The root of trust keys are provided through PKCS #11 APIs (KEY-ID-based APIs) that are executed in a trusted environment. 
- Examples are provided for the: STM32L4 Series, STM32F4 Series, STM32F7 Series, STM32G0 Series, STM32G4 Series, STM32H7 Series, STM32L0 Series, STM32L1 Series, and STM32WB Series of MCUs.

### X-CUBE-SBSFU security evaulation method description

- file:///C:/Users/Daniel%20Perez/Downloads/tn1387-xcubesbsfu-security-evaluation-method-description-stmicroelectronics.pdf
- The file was downloaded from the STM website and links to this documentation for how to implement secure boot on an STM board: file:///C:/Users/Daniel%20Perez/Downloads/dm00414687-getting-started-with-the-x-cube-sbsfu-stm32cube-expansion-package-stmicroelectronics.pdf. 
- Link to STM32 Trust: https://www.st.com/content/st_com/en/ecosystems/stm32trust.html
- This file seems to simply describe how STM hired an external auditor to verify their security features. The external auditor concluded that the product is reasonably secure as long as certain points were kept in mind: 
1. The integrator must follow the recommendations in the integration guide (see the AN5056 application note).
2. The solution must be deployed in RDP level 2 or physical countermeasures must be deployed to preventunauthorized debugging in level 1.
3. The integrator must individualize the SBSFU keys for each device and preserve the confidentiality of privateand secret keys.
4. The integrator must be careful when modifying the secure engine module, as a vulnerability in this module could lead to compromising the whole solution
- The documentation page downloaded does not provide much more detail about secure boot specifically that is not mentioned in the overview page. 

## Arduino Secure Boot

- https://blog.arduino.cc/2022/04/12/introducing-the-arduino-secure-boot/
- MCUBoot Github: https://blog.arduino.cc/2022/04/12/introducing-the-arduino-secure-boot/
- MCUBoot Docs: https://docs.mcuboot.com/
- The arduino framework seems to use something called: MCUBoot, as a method of offering firmware authentication and a secure firmware update method. It seems to be compatible with any hardware and operating system as long as the RTOes: zephyr, nuttx, mynewt, and mbed are used. 
- MCUboot requires configuration in order to enable, the basic configuration involves defining two flash areas; one flash area will contain the current application firmware image and the second will contain the update application image, which are slot 0 and slot 1 respectively. 
- MCUboot offers three different methods of switching between slots 0 and 1, overwrite (which is fastest but does not support rollback), swap scratch (which requires an extra flash area and wears the flash out more but offers roll back), and swap move (which uses extra space inside slot 0 and reduces wear factor of the flash). 
- MCU boot will also generate and store metadata using a tool called imgtool which processes the binary image, reserves the sapce, and adds all the required information for image verification. 
- Each image slot contains a header, the code or application binary, the TLV (which holds image hashes, key hashes, and image signatures, or other tuples with tag length and number), and the trailer (which stores swap status during image swaps). 
- MCUboot also provides an OTA design during which an update file is written to memory and processed by the bootloader, which updates the application. By default, swap scratch is used and firmware images and copies are encrypted by default. MCUboot will decrypt and update, take care of the needed offsets, then write it to the scratch area, while allowing for image rollback by re-encrypting the entire image before writing it. 
- MCUBoot uses imgtool to sign update images using a private key, while the public key is used by MCUboot to verify it. For image encryption, MCUboot uses the Elliptic curve integrated encryption scheme (ECIES) with a secp256r1 ephemeral keypair and a random AES key. Both keys are stored in flash alongside the bootloader binary. Note: the bootloader contains the board data, the encryption key, the signing key, and the boot id. 
- Unless MCUboot is configured, the bootloader will boot any sketch, but once keys are loaded, MCUboot will always verify the image signature and boot valid sketches, if an encrypted update is detected (by reading the TLV's), MCU will unwrap the encryption key and decrypt the image on-fhe-fly while moving it into the internal flash. 
