# 🔐 iOS Activation Wiki

> Technical resource for [r/SetupA12](https://www.reddit.com/r/SetupA12) — the community hub for iOS activation research.

**Live site →** https://tom-app-a1353ad1.base44.app

---

## How activation works (quick version)

```
Device erased
   │
   └─► POST to albert.apple.com
          ECID + IMEI + device certificate (TLS)
          │
          └─► Apple checks IMEI against Find My database
                 │
                 ├─ Flagged (Find My was on) → returns error → stuck on Hello screen
                 │
                 └─ Clear → returns signed activation record → device activates
```

The activation record is cryptographically bound to the device's **Secure Enclave UID key** — fused at manufacture, never readable by software.

---

## Chip × method matrix

| Chip | Devices | Bootrom exploit | Tethered activator | NAND rework |
|------|---------|-----------------|-------------------|-------------|
| A5 | iPhone 4S / iPad 2 | ✅ checkm8 | — | — |
| A6 | iPhone 5 / 5C | ✅ checkm8 | — | — |
| A7–A9 | iPhone 5S–6S / SE1 | ✅ checkm8 | — | — |
| A10 | iPhone 7 / iPad 6–7 | ✅ checkm8 | — | — |
| A11 | iPhone 8 / X | ✅ checkm8 (last) | — | — |
| A12 | iPhone XS / XR | ❌ Patched | ✅ Janus | ✅ |
| A13 | iPhone 11 series | ❌ Patched | ✅ Janus | ✅ |
| A14 | iPhone 12 series | ❌ Patched | ✅ Janus | ✅ |
| A15 | iPhone 13 / 14 | ❌ Patched | ✅ Janus | ✅ |
| A16+ | iPhone 15 / 16 | ❌ Patched | 🔬 Research | 🔬 Research |

**checkm8** = CVE-2019-8900 — unpatchable use-after-free in Apple BootROM USB DFU handler, disclosed by axi0mX, September 2019.

---

## Tool quick reference

| Tool | Chip | Signal | Cost | Platform |
|------|------|--------|------|----------|
| [Sliver 6.2](https://appletech752.com/downloads.html) | A5–A11 | ❌ | Free | Mac+Win |
| [F3arRa1n](https://f3arra1n.com) | A7–A11 | ❌ | Free/Paid | Windows |
| [iRemoval PRO](https://iremovalpro.com) | A7–A16 | ✅ | $12/device | Mac+Win |
| [iRemove Tools](https://iremove.tools) | A7–A11 | ✅ | Paid | Mac |
| [checkm8.info](https://checkm8.info) | A7–A11 | ❌ (free) | Free/Paid | Mac |
| [Janus](https://reddit.com/r/SetupA12) | A12–A15 | Partial | Free | Mac/Linux |
| [palera1n](https://github.com/palera1n/palera1n) | A8–A11 | N/A | Free | Mac/Linux |
| [PurpleSliver](https://appletech752.com/downloads.html) | A5–A6 iPad | ✅ | Free | Mac |

→ Full details: [docs/tool-matrix.md](docs/tool-matrix.md)

---

## Glossary (quick)

| Term | Meaning |
|------|---------|
| **checkm8** | Unpatchable BootROM exploit for A5–A11 (CVE-2019-8900) |
| **Ramdisk** | Temporary OS loaded into RAM to patch activation before iOS boots |
| **Tethered activation** | Bypass re-applied at every restart (A12+) |
| **NAND rework** | Physical chip modification for permanent A12+ bypass |
| **GSX** | Apple's Global Service Exchange — IMEI activation database |
| **albert.apple.com** | Apple's activation server endpoint |
| **Secure Enclave** | Hardware crypto chip; holds the UID key |
| **DFU mode** | Entry point for checkm8 exploitation |

→ Full glossary: [docs/glossary.md](docs/glossary.md)

---

## Community

| Community | Platform | Focus |
|-----------|----------|-------|
| [r/SetupA12](https://reddit.com/r/SetupA12) | Reddit | A12+ research, Janus, NAND |
| [r/setupapp](https://reddit.com/r/setupapp) | Reddit | General bypass |
| [AppleTech752](https://appletech752.com) | Website | Tools, matrix, guides |
| [XDA Developers](https://xda-developers.com) | Forum | Deep-dive research |

---

> ⚠️ **Disclaimer:** Educational and research resource. Never attempt bypass on a device you do not own.

*Maintained by [OutrageousStorm](https://github.com/OutrageousStorm) · CC BY-SA 4.0*
