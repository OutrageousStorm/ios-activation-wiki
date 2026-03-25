# Glossary

Key terms for iOS activation research.

---

**checkm8 (CVE-2019-8900)**  
An unpatchable use-after-free vulnerability in Apple's BootROM USB DFU handler. Affects every Apple SoC from A5 through A11. Disclosed by security researcher axi0mX in September 2019. Because it lives in read-only ROM, it cannot be patched by software updates on existing hardware.

**BootROM**  
The first code that runs when an Apple device powers on. Stored in read-only memory (ROM). It loads and cryptographically verifies iBoot, which in turn loads the kernel. checkm8 targets this stage.

**Ramdisk**  
A minimal iOS environment loaded entirely into RAM via DFU mode and the checkm8 exploit. Used to run commands or patch system files (like the activation daemon) before the main OS boots from NAND.

**Secure Enclave Processor (SEP)**  
A dedicated security coprocessor embedded in Apple SoCs from A7 onwards. Manages encryption keys, biometric data, and the UID key. Completely isolated from the main CPU — even Apple's firmware cannot read the UID key.

**UID Key**  
A unique 256-bit AES key fused into each device's Secure Enclave at manufacture. Never exposed to software. Used to derive per-device encryption keys, including those that protect activation data.

**Activation Record**  
A cryptographically signed ticket returned by Apple's activation server (albert.apple.com) confirming the device is activated. Stored on-device. Bypass tools either spoof this record or inject a fake one to satisfy iOS's activation check.

**Tethered Activation**  
A bypass that must be re-applied every time the device hard-reboots. The activation record is injected into memory and doesn't survive a full power cycle. The main approach for A12+ devices.

**NAND Rework**  
Physical modification of a device's NAND flash storage chip: desolder, read with a programmer, edit activation-related partitions, resolder. Achieves permanent factory-grade activation on A12+ devices. Requires microsoldering skills.

**GSX (Global Service Exchange)**  
Apple's internal database mapping device IMEIs to Apple IDs, warranty status, and activation locks. Legitimate carrier unlocks modify GSX directly. The authoritative source of activation status.

**albert.apple.com**  
Apple's activation server endpoint. Devices POST their ECID, IMEI, and device certificate here during setup. Returns a signed activation record if the device is not flagged.

**DFU Mode**  
Device Firmware Upgrade mode — a low-level state where the device accepts firmware instructions without loading iOS. The entry point for checkm8 exploitation and most bypass workflows.

**Kanzi Cable**  
A specialized hardware cable used by professional technicians to interface with Apple device NAND chips for reading, writing, and modification during hardware-level bypass work.

**MEID / GSM**  
Mobile Equipment Identifier (CDMA) and GSM device identifiers tied to the cellular modem. A bypass that restores "signal" means the cellular radio works — calls, texts, and mobile data function normally.

**Janus**  
An open-source tethered activator for A12+ devices (iPhone XS/XR through 15 Pro Max), released by the r/SetupA12 community. Works by injecting a spoofed activation record at each boot cycle.
