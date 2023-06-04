# Background Information

The exploring secure boot research project focuses on investigating how secure boot is implemented on various microcontrollers and embedded devices. 

**Secure Boot** is essentially a method of ensuring that only trusted or signed operating system code is executed on a device's bootup, it is primarily intended to prevent the injection or execution of malware in the stages before or between the firmware and the operating system exeuction [1]. This is because malware executed at this stage can circumvent other security systems in place at the operating system level. Secure boot is typically associated with traditional forms of computing on operating systems such as Windows, MacOS, Linux, etc., although the principles can be applied to embedded devices as long as their nature as memory-constrained devices are kept in mind. As such, secure boot on embedded devices has required solutions unique to the methods used in traditional computing. As mentioned in *"A Survey of of secure boot schemes for Embedded Systems"*[2], there are three main implementation methods: hardware-based, software-based, and hybrid methods. Hardware-based methods typically make use of Trusted Platform Computing. Software-based methods differ from hardware-based in that they typically require more overhead but no external hardware systems. Hybrid methods fit into neither category cleanly and typically use a combination of software or hardware-based methods. 

References: 
[1] https://uefi.org/sites/default/files/resources/UEFI_Secure_Boot_in_Modern_Computer_Security_Solutions_2013.pdf
[2] Wang, R., & Yan, Y. (2022). A Survey of Secure Boot Schemes for Embedded Devices. 2022 24th International Conference on Advanced Communication Technology (ICACT), 224–227. 

