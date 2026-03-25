# How iOS Activation Lock Works

Technical breakdown — from the server handshake to hardware exploitation.

---

## 1. The Activation Handshake

When a device is set up or erased, it contacts **albert.apple.com** over TLS. The request includes:
- Device ECID (unique chip ID)
- IMEI
- A device certificate signed by Apple

Apple's server checks these against its database and returns a signed **activation record** if the device is clear. Without this record, iOS refuses to proceed past the Hello screen.

---

## 2. Find My & the Lock Trigger

Activation Lock is armed automatically when **Find My iPhone** is enabled. The device's IMEI is registered against your Apple ID in Apple's GSX database. On erase, iOS checks this flag before requesting activation — if flagged, Apple's server will not issue a record regardless of what credentials are entered.

---

## 3. Secure Enclave & the UID Key

From iPhone 5s (A7) onwards, the **Secure Enclave Processor (SEP)** holds a unique 256-bit UID key fused into the chip at manufacture — never readable by software, even by Apple. Activation records are cryptographically bound to this key, which means you cannot clone or transfer an activation between devices.

---

## 4. checkm8: The Unpatchable BootROM Bug

In September 2019, security researcher **axi0mX** disclosed **CVE-2019-8900** — a use-after-free vulnerability in the USB Device Firmware Upgrade (DFU) handler in Apple's BootROM. It affects every Apple SoC from A5 through A11.

Because it's in **read-only ROM**, Apple can never patch it on existing hardware. This is the foundation of almost every free bypass tool for devices up to iPhone X.

How it works:
1. Device enters DFU mode
2. Exploit triggers a heap buffer overflow in the USB stack
3. Attacker gains code execution in BootROM context
4. Unsigned ramdisk can be loaded

---

## 5. Ramdisk Bypass (A7–A11)

Using checkm8, tools like Sliver and F3arRa1n boot a custom **ramdisk** — a minimal iOS environment loaded entirely into RAM. This ramdisk patches the activation validation daemon (`com.apple.mobileactivationd`) before the main iOS system loads, allowing the device to reach the home screen without a valid Apple ID.

The patch is **untethered** — it persists across soft reboots. Cellular signal is typically not restored in free implementations; paid tools like iRemoval PRO and iRemove Tools address this with additional server-side activation.

---

## 6. Tethered Activation (A12+)

Apple's **Secure Boot** chain on A12+ cryptographically verifies every stage of the boot process. BootROM exploits are impossible.

Tethered activators like **Janus** work around this by injecting a spoofed activation record into memory at each boot cycle. The device works normally during that session, but requires reconnecting to a computer and re-running the activator after a hard restart.

---

## 7. NAND Rework & Hardware Methods

For A12+ devices requiring a **permanent, signal-capable** solution, technicians physically:
1. Desolder the NAND flash chip
2. Read its contents with a NAND programmer (using a Kanzi cable or equivalent)
3. Modify the activation-related partition data
4. Resolder the chip

When done correctly, the device activates as if it were factory-fresh. Requires expert-level microsoldering skill and expensive equipment.

---

## 8. GSX & Server-Side Unlocks

Apple's **Global Service Exchange (GSX)** is the backend database that maps IMEIs to Apple IDs and activation status. Carrier unlock requests modify this database legitimately. A genuine GSX unlock is permanent and survives restores — it's the gold standard.

Unofficial routes into GSX have historically existed but Apple has continuously closed them.
