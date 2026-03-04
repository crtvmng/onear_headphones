## 📖 Overview
[![PCB: Altium Designer](https://img.shields.io/badge/PCB-Altium%20Designer-A5915F?style=flat-square&logo=altiumdesigner&logoColor=white)](.)
[![MCU: STM32F446](https://img.shields.io/badge/MCU-STM32F446-03234B?style=flat-square&logo=stmicroelectronics&logoColor=white)](.)
[![USB: Audio Class 2.0](https://img.shields.io/badge/USB-Audio%20Class%202.0-5C6BC0?style=flat-square)](.)
[![Hi-Res: PCM 384kHz](https://img.shields.io/badge/Hi--Res-PCM%20384kHz%20%2F%20DSD-c8a96e?style=flat-square)](.)
[![Status: In Development](https://img.shields.io/badge/Status-In%20Development-orange?style=flat-square)](.)

This project is a fully custom-designed, portable high-resolution audio device combining a flagship **ESS Sabre DAC**, an ultra-low-noise power architecture, and a high-current amplification stage into a single compact PCB. It is built from the ground up to handle the unique electrical demands of planar magnetic headphone drivers — low impedance, high current, and wide bandwidth.

The design targets:

* 🎧 **Planar magnetic headphones** (HiFiMAN, Audeze, Fostex RP-series)
* 💻 **USB Audio Class 2.0** — driverless on macOS/Linux, ASIO on Windows
* 🔋 **Portable operation** — Li-Po battery with smart power-path management
* 📐 **DIY / open-source** — fully documented schematic & PCB in Altium Designer

--- 
## ⚡ Key Components 
### Core Processing & Communication 
| Block | Component | Role | 
|---|---|---| 
| *MCU* | STM32F446 (ARM Cortex-M4 @ 180MHz) | USB Audio 2.0 host, I2S master, firmware | 
| *DAC* | ESS Sabre ES9039Q2M | 32-bit stereo, HyperStream® IV, −122 dB THD+N | 
| *Clock* | Si5351A | External master clock, eliminates MCU jitter | 
| *Amplifier* | TI OPA1622 (SoundPlus™) | 145 mA output current, drives planar magnetics |

---
### Power Architecture 
| Rail | Component | Spec |
|---|---|---| 
| *Analog LDO* | LT3045 | Ultra-low noise (2 µV RMS), 76 dB PSRR | 
| *Power-path* | BQ24072 | USB ↔ Li-Po seamless switching | 
| *Soft-start LDO* | TLV75533 | PWR\_HOLD logic, battery protection | 
| *ESD Protection* | USBLC6-2 | USB port transient protection | 

--- 
## 🔬 Technical Highlights 
### Planar-Ready Amplification 
The **OPA1622** output stage is configured with a bipolar supply rail (±) to provide the full voltage swing and instantaneous current delivery needed by low-impedance planar membranes. Unlike dynamic drivers, planars require the amplifier to maintain grip across the entire frequency range without current clipping. 
### Precision Clocking 
The **Si5351A** external clock generator feeds a dedicated, jitter-cleaned MCLK directly to the ES9039Q2M, bypassing the STM32's internal PLL. This ensures bit-perfect audio synchronisation for high-resolution PCM (up to 384 kHz) and native DSD playback. 
### Analog Power Isolation 
The **LT3045** ULNLDO is dedicated exclusively to the DAC's AVCC and AVCC33 rails. Its extraordinary PSRR (>80 dB at 1 MHz) and ultra-low output noise floor (2 µV RMS) prevent any switching activity on the digital plane from coupling into the audio signal path. 
### PCB Layout Strategy 
- ✅ **Split ground planes** — Digital (DGND) and Analog (AGND) planes joined at a single star point via NetTie
- ✅ **Series termination resistors** on all I2S / MCLK / BCLK lines to suppress reflections and EMI
- ✅ **C0G/NP0 dielectrics** for all analog filter capacitors (no piezoelectric distortion) -
- ✅ **Low-ESR polymer/tantalum caps** for power supply decoupling -
- ✅ **Guard rings** around DAC analog outputs

--- 
## 📋 Specifications 
| Parameter | Value |
|---|---|
| **USB Interface** | USB Type-C, Audio Class 2.0 (Async) |
| **Max PCM Resolution** | 32-bit / 384 kHz | 
| **DSD Support** | DSD64 / DSD128 (DoP) | 
| **THD+N (DAC)** | −122 dB (typical) |
| **Output Current (Amp)** | 145 mA (peak) |
| **Output Impedance** | < 1 Ω |
| **Headphone Outputs** | 4.4 mm Balanced + 3.5 mm Single-ended | 
| **Power Source** | USB 5V / Li-Po 3.7V (via BQ24072) |
| **SNR** | > 120 dB |

---

## 🚀 Getting Started

### Prerequisites

- **Altium Designer** 22+ (for hardware files)
- **STM32CubeIDE** (for firmware)
- **ST-Link V2** or compatible SWD programmer

### PCB Fabrication Notes

> ⚠️ This board requires a **4-layer stackup** for proper impedance control and plane separation.

| Layer | Function |
|---|---|
| **L1 (Top)** | Signal (USB, I2S, analog out) |
| **L2** | Solid GND plane |
| **L3** | Power plane (split DGND / AGND) |
| **L4 (Bottom)** | Signal (power routing, decoupling) |

Recommended: **JLCPCB / PCBWay** with ENIG finish, 1 oz copper, controlled impedance (90 Ω differential for USB lines).

---

## 🧩 Compatible Headphones

| Model | Impedance | Sensitivity | Notes |
|---|---|---|---|
| HiFiMAN HE400se | 25 Ω | 91 dB/mW | ✅ Excellent match |
| Audeze LCD-2 | 70 Ω | 101 dB/mW | ✅ Excellent match |
| Fostex T50RP Mk3 | 50 Ω | 92 dB/mW | ✅ Good match |
| Audeze LCD-4 | 200 Ω | 97 dB/mW | ✅ Works on balanced out |

---

## 📌 Status & Roadmap

- [x] Schematic design — Rev A
- [x] Component selection & BOM
- [ ] PCB layout — in progress
- [ ] Rev A fabrication & bring-up
- [ ] Firmware: USB Audio 2.0 enumeration
- [ ] Firmware: ES9039Q2M register map & init
- [ ] Firmware: Si5351A clock configuration
- [ ] Listening tests & measurements (REW / APx)
- [ ] Rev B (post-bringup corrections)

---


## License
This project is licensed under the **CERN Open Hardware License v2 - Weakly Permissive (CERN-OHL-W-2.0)**. 
For more details, see the [LICENSE](LICENSE) file.

---

<div align="center">
*Built with obsessive attention to analog signal integrity.*
<br/>
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:16213e,50:1a1a2e,100:0a0a0a&height=80&section=footer" />
</div>
