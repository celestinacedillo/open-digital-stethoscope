# Digital Stethoscope — RevD_E73

Open hardware for a battery-powered digital stethoscope board. A MEMS microphone feeds a low-noise preamp and a 24-bit audio ADC. A Bluetooth Low Energy module (Ebyte E73, nRF52832) streams or records the signal, and a USB-C charger runs a single Li-ion cell.

This is **revision D (RevD_E73)**. It passes JLCPCB's online DFM checks (PCB and SMT assembly) with zero errors and is ready to order. It has **not yet been built and tested**, so treat it as an unverified release.

## Board at a glance

| Item | Value |
| --- | --- |
| Size / shape | 50 mm round, flat edge for the side switch |
| Stack-up | 4 layers, FR-4, 1.6 mm; inner layer 1 = solid GND |
| Assembly | All parts on top; 34 BOM lines / 76 placed parts |
| Vias | 0.3 mm drill, 0.61 mm pad (JLCPCB standard tier) |
| Test points | TP1–TP6 on the bottom as bare pogo pads (not assembled) |

## Main parts

| Function | Ref | Part | LCSC |
| --- | --- | --- | --- |
| BLE MCU module | U8 | Ebyte E73-2G4M08S1C (nRF52832) | C356849 |
| Microphone | MK1 | Infineon IM73A135V01 (bottom-port MEMS) | C3171831 |
| Mic preamp | U6 | TI OPA2320AIDR | C2863402 |
| Audio ADC | U2 | TI PCM1808PWR | C55513 |
| Analog LDO (2.8 V) | U7 | TI LP5907MFX-2.8 | C186700 |
| Li-ion charger / power path | U9 | TI BQ24075RGTR | C15464 |
| Buck-boost (3V3) | U10 | TI TPS63001DRCR | C28060 |
| Buck-boost (5 V analog) | U5 | TI TPS63002DRCR | C116655 |
| Load switch | U4 | TI TPS22918DBVR | C131941 |
| USB-C + ESD | J1, D1 | TYPE-C-31-M-12, USBLC6-2SC6 | C165948, C7519 |
| Battery connector | J2 | JST SM02B-SRSS-TB (1.0 mm) | C160402 |

The full list is in [`assembly/BOM_RevD_E73_JLCPCB.csv`](assembly/BOM_RevD_E73_JLCPCB.csv).

## Repository layout

| Path | Contents |
| --- | --- |
| `fabrication/Gerber_RevD_E73.zip` | Gerbers and drill files for the PCB order |
| `assembly/BOM_RevD_E73_JLCPCB.csv` | BOM with LCSC part numbers (JLCPCB format) |
| `assembly/CPL_RevD_E73_JLCPCB.csv` | Pick-and-place file (JLCPCB format) |
| `docs/Schematic_RevD_E73.pdf` | Schematic, 5 pages |
| `source/easyeda/RevD_E73_PCB.epro2` | EasyEDA Pro PCB source |
| `mechanical/RevD_E73.step` | 3D model of the assembled board, for enclosure design |

## Ordering from JLCPCB

1. Upload `fabrication/Gerber_RevD_E73.zip`. Choose 4 layers, 1.6 mm, and the standard 0.3 mm via option.
2. Enable SMT assembly (top side) and upload the BOM and CPL from `assembly/`.
3. Check part rotations in JLC's placement preview, especially U2, U5, U9, U10, D1, LED2 and MK1.

**Stock warning:** check stock for U8 (E73-2G4M08S1C, C356849) before ordering. On 19 Nov 2026 JLCPCB had only 1 in stock.

## DFM status (JLCDFM, 19 Nov 2026)

| Check | Errors | Warnings left | Why the warnings stay |
| --- | --- | --- | --- |
| PCB DFM | 0 | 16 "PTH without pad" | 8 holes that must stay unplated: 3 mounting holes, the mic sound port, 2 USB-C locating pegs, 2 SW1 locating pegs (each counted top and bottom) |
| SMT DFM | 0 | 2 "Component clipped by board outline" | The USB-C socket and the side switch overhang the edge by design |

## Mechanical notes

- The USB-C socket front overhangs the board edge by about 0.8 mm. The enclosure needs a matching opening.
- The SW1 slide-switch lever sticks out past the flat edge and needs a slot in the case.
- The microphone hears through a 0.8 mm hole under MK1, so keep that path open on the back.
- The three Ø2.2 mm mounting holes are unplated and not connected to any net.

## Firmware notes

- PGOOD_N (P1.13) needs the internal pull-up enabled.
- NRST is on P0.18; enable pin reset in UICR.
- The button on P0.15 is a user/DFU button with an external 10 kΩ pull-down (press = 3V3). There is no hardware boot-mode pin.
- LED2 is the charge indicator. It is driven by the charger's CHG_N output, and the MCU can also pull it low (open-drain only).
- The charger's TS pin uses a fixed 10 kΩ resistor (no NTC), so there is **no battery temperature protection**.

## Safety

This is an engineering prototype, not a medical device. It has not been tested, certified or cleared by any regulator for clinical use. Use it at your own risk.

## License

Copyright © 2026 The project contributors.

This source describes Open Hardware and is licensed under the CERN-OHL-S v2. See [`LICENSE`](LICENSE).

You may redistribute and modify this source and make products using it under the terms of the [CERN-OHL-S v2](https://ohwr.org/cern_ohl_s_v2.txt). This source is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN-OHL-S v2 for applicable conditions.

Source location: add this repository's URL here after publishing. Under section 4.1 of the licence, products made from this source should carry this Source Location wherever practicable, for example on the PCB silkscreen or in the product documentation.
