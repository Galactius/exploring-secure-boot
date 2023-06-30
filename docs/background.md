# Background Information (Rev. 2) 

The exploring secure boot research project focuses on investigating the implmentation and impacts of secure boot on varous embedded systems, primarily microcontrollers that do not use an operating system. 

## What is **Secure Boot**? 

In the context of traditional computer systems, **Secure Boot** is essentially a method of ensuring that only trusted or signed operating system code is executed during a device's boot sequence, it is primarily intended to prevent the injection or execution of malware in the stages before or between the firmware and the operating system exeuction [1]. This is because malware executed at this stage can circumvent other security systems in place at the operating system level, such as traditional anti-virus or anti-malware software. Secure boot is typically associated with traditional forms of computing on operating systems such as Windows, MacOS, Linux, etc., although the principles can be applied to embedded devices as long as the nature of embedded devices as memory-constrained devices are kept in mind. As such, secure boot on embedded devices has required solutions unique to the methods used in traditional computing. As mentioned in *"A Survey of of secure boot schemes for Embedded Systems"* [2], there are three main implementation methods: hardware-based, software-based, and hybrid methods. Hardware-based methods make use of some hardware element to ensure trust between boot phases. Software-based methods differ from hardware-based in that they typically require more overhead but no external hardware systems. Hybrid methods fit into neither category cleanly and typically use a combination of software or hardware-based methods.

Generally, the boot process of most devices are split into mulitlple phases, with two different bootloaders. The first bootloader is often built into the device's read-only memory (ROM) and is either assumed to be trusted from the factory or has some sort of hardware component used to validate that it has not been tampered with. The first bootloader will then execute some sort of code to ensure that the second bootloader is valid or has not been tampered with (either by verifying cryptographically generated hashes or other methods that establish trust). Once the second bootloader has been verified, it is used to verify the user-flashed application firmware. If the second bootloader is not trusted (the hashes do not match, or other method fails), then the user-flashed application firmware is not executed, thus ensuring that the invalid or maliscious code has not been executed. 

## Secure Boot Implementations 

As previously mentioned, there are various methods of implementing secure boot, although most methods typically require some sort of element, there is a way to implement secure boot through software. Most consumer and commercial grade platforms make use of a [Trusted Platform Module](#trusted-platform-modules) as a method to establish trust. 

Software based secure boot implementations are those that do not use hardware as the primary method of achieving trust. When using software-based method, an assumption of trust is often made in that the first bootloader (stored in ROM from the factory) or the set of secure operations used to establish trust are reliable. These implementations are often also less secure as a result, but can be more easily applied to devices that do not have existing hardware dedicated to security. 

## Software-based Secure Boot

The most common implementations of software secure boot I have seen involve: virtualization where isolation is used to allow security operations to occur in a secure environment, a "measured boot" in which the cryptographic hash of the current image/bootloader is compared to a known valid hash, a "verified boot", or some other method of creating a Trusted Execution Environment (TEE) where operations used to establish trust are performed. 

The virtualization method for implementing software-based involves creating a separate, isolated environment in which trusted or secure operations can occur. This secondary virtualized partition is then assumed to be trusted to execute code that implements security features or manage crytographic operations. ARM's trustZone technology uses a similar framework, but requires specialized hardware to enable security features.

The "measured boot" approach involves storing the cryptographic hashes of the images and bootloaders used in the boot sequence in some sort of secure or trusted portion of memory, then comparing those hashes to a known valid hash either through some validation done on the device, or by connecting to an external service which validates the cryptographic hash. 

The "verified boot" approach is very similar to the general framework for secure boot, but does not make use of external hardware and instead relies on extablishing trust outside of the device. Essentially, the keys used to sign and encrypt the bootloader are generated and stored offsite (usually by the manufacturer of the device/program). This key is then used to verify the hash of the existing firmware. [9]. 

The final and most generalized method for software based secure boot is to create some sort of Trusted Execution Environment in which security operations can occur. This is most likely either done through code stored in ROM or through access to an external service or hardware module. This TEE must then manage crytopraphic operations that will be used during secure boot. 

## Trusted Platform Modules

The use of trusted platform modules are the most common form of adding secure boot functionality (as well as other security features) on devices that do not already have a secure boot core built-in to the SoC or CPU (as such this is *external hardware*). This is becuase TPMs provide a hardware root-of-trust through the generation of crytographic keys as well as other Trusted Execution features. TPMs are a technology developed through various specifications published by the Trusted Computing Group [6]. The entire TPM specification is written out through four documents. There are also various forms of Trusted Platform Modules as defined by the Trusted Computing Group's TPM 2.0 overview [7]: 

1. Discrete TPMs (dTPM) - these are typically external TPM hardware modules (as described in the [External-Hardware-Based Secure Boot](#external-hardware-based-secure-boot) section). 
2. Integrated TPMs (iTPM) - these are similar to dTPMs in that operations occur within a seperate module to the SoC/CPU, but the module may not be fully dedicated to security features. 
3. Firmware TPMs (fTPM) - this form of TPM is similar to the implementations described in the [Internal-Hardware-Based Secure Boot](#on-internal-hardware-based-secure-boot) section in that security features are executed on the CPU in a Trusted Execution Environment. 
4. Software TPMs (sTPM) - these are some sort of software emulation of a TPM and provide the least protection, but are often used to test or debug TPM systems for future products. 

Generally, devices that do not make use of a Trusted Platform Module make use of some sort of internal hardware block or module that provides similar features. 

## On Internal-Hardware-based Secure Boot

Internal-Hardware-based secure boot implementations primarily come in two forms: a hardware block dedicated to crytographic operations or the management of keys, or a secondary general purpose processing unit that executes secure operations [3] both of which are included with the main System on a Chip (SoC) or processor. Regardless of which method is used, **the main purpose of using a hardware implementation is to establish trust between firmware operations and high-level operating system function calls** which can be done by using cryptographic keys and signatures to verify code integrity or through other means. The benefits with hardware-based solutions include: high physical security and the ease of implementing anti-tampering mechanisms. However, the main drawbacks include the cost of design and integration into new systems (as the hardware must be included in the design of a new product or SoC) and the inflexible nature of hardware components especially for ease of integrating improvements or updates. Two significant issues that arise with using hardware-based secure boot solutions include the fact that the passage of secure information to insecure places in memory can risk the integrity of the secure information if proper precautions are not taken. Another prevalent issue comes from debug interfaces in that hardware solutions may require use of JTAG or UART debugging interfaces, but they also cannot be left enabled on an end-device. The usage of debug interfaces to access the firmware on a device is a common practice and can lead to an end-user accessing information that could risk the integrity of the device.

In the case of the first case with a hardware-block, a **root of trust** is established using a set of encryption keys either generated by and stored on the device, generated by the manufacturer of a device, or a combination of both. These encryption keys are then used to sign files, which means the signed file is verified or trusted by the hardware-block. Different microcontroller platforms make use of different encryption schemes, such as Arduino's MCUBoot secure boot solution using using the Elliptic Curve Integrated Encryption Scheme [4] or Espress-ifs ESP32 series making use of AES based encryption keys in newer implementations of secure boot [5]. A common problem with hardware-block based solutions is the risk of compromised encryption keys. If keys cannot be regenerated or revoked, attacks that target compromising encryption keys could render these defenses useless.

In implementations that make use of a secondary general-purpose processor, all secure operations occur within the secondary processor, then some sort of interface or API must be used to retrieve secure information to an insecure location. This particular design is slightly more difficult to implement as adding a secondary processor takes up valuable silicon die space that could be used for additional or expanded peripherals.

## References:

1. https://uefi.org/sites/default/files/resources/UEFI_Secure_Boot_in_Modern_Computer_Security_Solutions_2013\.pdf
2. Wang, R., & Yan, Y. (2022). A Survey of Secure Boot Schemes for Embedded Devices. 2022 24th International Conference on Advanced Communication Technology (ICACT), 224–227\. 
3. (Arm Trustzone Whitepaper)
4. https://blog.arduino.cc/2022/04/12/introducing-the-arduino-secure-boot/
5. https://blog.espressif.com/esp32-s2-security-improvements-5e5453f98590
6. https://trustedcomputinggroup.org/wp-content/uploads/TCG_TPM2_r1p59_Part1_Architecture_pub.pdf
7. https://www.trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf
8. https://learn.microsoft.com/en-us/windows/security/operating-system-security/system-security/secure-the-windows-10-boot-process
9. https://lwn.net/Articles/571031/
