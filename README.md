# ASUS VivoBook 16X Hackintosh (OpenCore)

![macOS Version](https://img.shields.io/badge/macOS-Tahoe%20%7C%20Sonoma%20%7C%20Sequoia-black?style=for-the-badge&logo=apple)
![OpenCore](https://img.shields.io/badge/OpenCore-1.0.8-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Working%20with%20Iris%20Xe%20Acceleration-success?style=for-the-badge)

OpenCore EFI configuration for the **ASUS VivoBook 16X** powered by the **12th Gen Intel Core i5-12500H (Alder Lake-P)** with working **Intel Iris Xe Graphics (80 EU)** acceleration.

---

## 💻 Hardware Specifications

| Component | Details |
| :--- | :--- |
| **Model** | ASUS VivoBook 16X (K3605 / X1605 Series) |
| **Processor (CPU)** | Intel Core i5-12500H (12 Cores / 16 Threads: 4P + 8E) |
| **Integrated Graphics (iGPU)** | Intel Iris Xe Graphics (80 EU) - Spoofed as Tiger Lake `0x9A49` |
| **Dedicated Graphics (dGPU)** | NVIDIA GeForce RTX (Disabled via `SSDT-dGPU-Off.aml`) |
| **Display** | 16.0" 16:10 WUXGA (1920x1200) @ 120Hz (`N160JME-GL2`) |
| **RAM** | 24 GB DDR4 3200 MHz |
| **Audio** | Realtek ALC Layout 11 (`alcid=11` / AppleALC + SimpleHDA) |
| **Touchpad & Keyboard** | AsusSMC + VoodooI2C + VoodooPS2Controller |
| **SMBIOS** | `MacBookPro16,2` |

---

## 📊 What Works & What Doesn't

- [x] **Full Graphics Acceleration** (Metal 2, Quartz Extreme, Core Animation via NootedGreen / WhateverGreen patches)
- [x] **120Hz Refresh Rate** on internal 16:10 display
- [x] **CPU Power Management & Turbo Boost** (Hybrid P+E cores handled via `CpuTopologyRebuild`)
- [x] **Internal Audio & Microphone**
- [x] **Brightness Keys & Display Backlight Control**
- [x] **Battery Status & Power Sensor Readings** (AsusSMC)
- [x] **Trackpad with Multi-touch Gestures**
- [x] **USB Ports** (Mapped)
- [x] **Sleep & Wake**
- [ ] **NVIDIA dGPU** (Unsupported on macOS, safely powered down to save battery)
- [ ] **Intel Wi-Fi / Bluetooth** (Use `AirportItlwm.kext` matching your macOS version)

---

## ⚠️ Important: Generate Your Own SMBIOS Serials!

All personal serial numbers have been stripped from this EFI to prevent Apple ID / iCloud / iMessage account blacklisting. **You must generate new serials before booting.**

### Steps:
1. Download **[GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)**.
2. Select option `Generate SMBIOS` and choose model:
   ```text
   MacBookPro16,2
   ```
3. Open `EFI/OC/config.plist` using **ProperTree** or **OCAuxiliaryTools**.
4. Navigate to `PlatformInfo` ➔ `Generic` and fill in:
   - `SystemProductName`: `MacBookPro16,2`
   - `SystemSerialNumber`: (Your generated Serial Number)
   - `MLB`: (Your generated Board Serial Number)
   - `SystemUUID`: (Your generated UUID or run `uuidgen` in terminal)
   - `ROM`: (Your physical MAC address or 6 random bytes)
5. Save the file.

---

## 🛠 BIOS Settings

### Recommended Settings:
- **Fast Boot:** Disabled
- **Secure Boot:** Disabled
- **VT-d:** Enabled (or Disabled if experiencing kernel panics)
- **VT-x / Intel Virtualization:** Enabled
- **SATA Mode:** AHCI
- **Graphics / DVMT Pre-Allocated:** 64MB or above (if accessible)

---

## 📁 Repository Structure

```text
├── EFI/
│   ├── BOOT/
│   │   └── BOOTX64.EFI
│   └── OC/
│       ├── ACPI/                     # DSDT patches (dGPU disable, EC, ALS0, PLUG-ALT)
│       ├── Drivers/                  # OpenRuntime, OpenCanopy, OpenHfsPlus
│       ├── Kexts/                    # AsusSMC, NootedGreen, VoodooI2C, Lilu, etc.
│       ├── Resources/                # Audio & GUI Themes
│       ├── config.plist              # Main configuration (Metal Acceleration ON)
│       ├── config-install.plist      # Safe / Verbose config for initial installation
│       └── config-desktop-120hz.plist# Config specifically tuned for 120Hz display
└── README.md
```

---

## 👏 Credits & Acknowledgments

- **[Acidanthera](https://github.com/acidanthera)** for OpenCorePkg, Lilu, AppleALC, VirtualSMC, and documentation.
- **[sgiammori / NootedGreen Team](https://github.com/sgiammori/NootedGreen)** for Intel Iris Xe acceleration research and drivers.
- **[b00t0x](https://github.com/b00t0x)** for WhateverXe.
- **[hieplpvip](https://github.com/hieplpvip/AsusSMC)** for AsusSMC.
- The Hackintosh community for continuous testing and feedback.
