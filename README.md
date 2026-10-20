# AUX-to-Bluetooth Audio Transmitter

A custom PCB I designed in KiCad that adds Bluetooth to anything with a headphone jack. Plug in an AUX cable, power it over USB-C, and stream to your wireless headphones.

<img width="927" height="317" alt="image" src="https://github.com/user-attachments/assets/9188d425-4a66-4c4d-9c49-06d7ead04fde" />

## Table of Contents
- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Schematic & PCB](#schematic--pcb)
- [Firmware](#firmware)
- [Getting Started](#getting-started)
- [Ordering the Board](#ordering-the-board)
- [Bill of Materials (BOM)](#bill-of-materials-bom)
- [License](#license)

## Overview

Lots of good gear has no Bluetooth: an old TV, a record player, a treadmill, a car stereo with an AUX port. I wanted one small board that fixes that, so I designed it myself, from the schematic to the PCB layout.

### What it does

- **Wired to wireless:** Takes analog audio from a standard 3.5 mm AUX cable.
- **Clean conversion:** A 24-bit stereo ADC turns the sound into digital audio.
- **Bluetooth streaming:** An ESP32 sends the audio to your headphones or speaker.
- **Long range:** An RP-SMA connector lets you attach an external antenna.

### Real-world examples

- **Old TV:** Watch late at night with AirPods, no cable across the room.
- **Record player:** Listen to vinyl on wireless headphones.
- **Gym treadmill:** No cable in your way while you run.

## Schematic & PCB

Audio goes in through the AUX jack, gets digitized, and is sent out over Bluetooth:

```
AUX jack → PCM1808 (ADC) → ESP32 (Bluetooth) → your headphones
```
<img width="1337" height="637" alt="image" src="https://github.com/user-attachments/assets/cfc3fada-e944-4981-83fc-2ea981cc686d" />
<img width="806" height="392" alt="image" src="https://github.com/user-attachments/assets/4a6c198e-a7d2-4a69-a337-82b32398226d" />

| Part | What it does |
|---|---|
| ESP32-WROOM-32UE-N4 | The brain: Bluetooth pairing and streaming |
| PCM1808PWR | Turns analog sound into digital (I2S) audio |
| AP2112K-3.3 | Steps USB 5 V down to a steady 3.3 V |
| CH340C | USB-to-serial chip so you can upload code over USB-C |
| SJ-3523-SMT-TR | 3.5 mm stereo input jack |

---

## Firmware

The ESP32 needs code before the board will do anything. You upload it once with the Arduino IDE.

- **Code:** [firmware.ino](https://github.com/Shashvat1333/Bluetooth-Audio-Transmiter/blob/main/firmware.ino)
- **Board:** ESP32 Dev Module
- **Port:** USB-C (this one port both powers the board **and** uploads the code)

---

## Getting Started

1. **Download the firmware:** Grab the code from the [firmware.ino](https://github.com/Shashvat1333/Bluetooth-Audio-Transmiter/blob/main/firmware.ino).
2. **Set up the Arduino IDE:** Install the ESP32 board package (Boards Manager → search "esp32").
3. **Connect the board:** Plug the board into your computer with a USB-C cable. This one cable powers the board **and** uploads the code.
4. **Select the board:** Tools → Board → **ESP32 Dev Module**, then pick the correct port under Tools → Port.
5. **Upload:** Open the firmware file and click **Upload**.
6. **Plug in your audio:** Connect your audio source to the 3.5 mm AUX jack.
7. **Pair:** Put your headphones in pairing mode. The board connects automatically.

> **Tip:** If your computer doesn't show a port, install the CH340 USB driver and try a different USB-C cable (some are charge-only).

After the first upload, you only need USB-C power. Any phone charger works.

---

## Ordering the Board

Zip the `Gerbers` folder and upload it to your PCB maker (JLCPCB, PCBWay, etc.) with these settings:

| Setting | Value |
|---|---|
| Layers | [2] |
| Board size | [110 x 30 mm] |
| Thickness | [1.6 mm] |
| Copper | [1 oz] |
| Surface finish | [HASL lead-free] |
| Solder mask | [Green] |

---

## Bill of Materials (BOM)

| Part | What it's for | Qty | Unit | Total | Vendor |
| --- | --- | --- | --- | --- | --- |
| [UJ20-C-H-G-SMT-1-P16-TR](https://jlcpcb.com/partdetail/CUI-UJ20_C_H_G_SMT_1_P16TR/C45082766) | Use to power and program the esp32 | 1 | $0.86 | $0.86 | [JLCPCB](https://jlcpcb.com/partdetail/CUI-UJ20_C_H_G_SMT_1_P16TR/C45082766) |
| [Ceramic Capacitor Assortment Kit](https://www.aliexpress.com/item/1005012290380278.html?spm=a2g0o.productlist.main.2.328544d8SuTRhb&algo_pvid=40487480-af34-4fa0-991f-8491ed68449b&algo_exp_id=40487480-af34-4fa0-991f-8491ed68449b-1&pdp_ext_f=%7B%22order%22%3A%22786%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%2111.33%219.88%21%21%2152.17%2145.49%21%402103128817917564117867231e1157%2112000057969079635%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000218598348&curPageLogUid=g6HIOIDwDsUK&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005012290380278%7C_p_origin_prod%3A) | Ceramic capacitors are tiny passive electronic components used here to filter out electrical noise on power rails and block DC voltage while letting audio signals pass through. | 1 | $13.64 | $13.64 | [Aliexpress](https://www.aliexpress.com/item/1005012290380278.html?spm=a2g0o.productlist.main.2.328544d8SuTRhb&algo_pvid=40487480-af34-4fa0-991f-8491ed68449b&algo_exp_id=40487480-af34-4fa0-991f-8491ed68449b-1&pdp_ext_f=%7B%22order%22%3A%22786%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%2111.33%219.88%21%21%2152.17%2145.49%21%402103128817917564117867231e1157%2112000057969079635%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000218598348&curPageLogUid=g6HIOIDwDsUK&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005012290380278%7C_p_origin_prod%3A) |
| [AP2112K-3.3TRG1](https://jlcpcb.com/partdetail/DiodesIncorporated-AP2112K_33TRG1/C51118) | The AP2112K-3.3TRG1 is a low-dropout (LDO) voltage regulator used here to step down your incoming 5V power supply to a steady, clean 3.3V to power the ESP32 microcontroller and digital logic pins. | 1 | $0.19 | $0.19 | [JLCPCB](https://jlcpcb.com/partdetail/DiodesIncorporated-AP2112K_33TRG1/C51118) |
| [ESP32-WROOM-32UE-N4](https://jlcpcb.com/partdetail/EspressifSystems-ESP32_WROOM_32UEN4/C701344) | The ESP32 is a microcontroller used here to run firmware that receives the digital I2S audio stream from the PCM1808 and handles Wi-Fi or Bluetooth connectivity. | 1 | $4.50 | $4.50 | [JLCPCB](https://jlcpcb.com/partdetail/EspressifSystems-ESP32_WROOM_32UEN4/C701344) |
| [CH340C](https://www.aliexpress.com/item/1005007206307679.html?spm=a2g0o.productlist.main.2.14735d38jJiEP4&algo_pvid=08c2775b-a139-455d-8b44-be4d4204f0a8&algo_exp_id=08c2775b-a139-455d-8b44-be4d4204f0a8-1&pdp_ext_f=%7B%22order%22%3A%2220%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%214.39%214.39%21%21%213.02%213.02%21%402101c28d17917628228577887e0c6e%2112000039805627300%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895&curPageLogUid=tDRd9NJte0Jp&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005007206307679%7C_p_origin_prod%3A) | The CH340C is a USB-to-UART bridge IC used here to let your computer communicate with the ESP32 for programming and debugging via a standard USB-C cable. | 1 | $4.20 | $4.20 | [Aliexpress](https://www.aliexpress.com/item/1005007206307679.html?spm=a2g0o.productlist.main.2.14735d38jJiEP4&algo_pvid=08c2775b-a139-455d-8b44-be4d4204f0a8&algo_exp_id=08c2775b-a139-455d-8b44-be4d4204f0a8-1&pdp_ext_f=%7B%22order%22%3A%2220%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%214.39%214.39%21%21%213.02%213.02%21%402101c28d17917628228577887e0c6e%2112000039805627300%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895&curPageLogUid=tDRd9NJte0Jp&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005007206307679%7C_p_origin_prod%3A) |
| [[NC]2.4GHz 3dBi WiFi 2.4g Antenna +15cm Aerial RP-SMA Male Wireless Router PCI U.FL IPX To RP SMA Male Pigtail Cable[NC]](https://www.aliexpress.com/item/1005012839987115.html?spm=a2g0o.productlist.main.17.22487e4aAc5l2Y&algo_pvid=5c910e90-451d-4eb2-b6c2-a9b67833a777&algo_exp_id=5c910e90-451d-4eb2-b6c2-a9b67833a777-16&pdp_ext_f=%7B%22order%22%3A%228%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%2112.90%215.00%21%21%2159.41%2123.03%21%402101e80b17915677645397405e0de8%2112000059523438374%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000216666461&curPageLogUid=c4JAZG0wstCM&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005012839987115%7C_p_origin_prod%3A) | That antenna and pigtail cable are used here to connect an external 2.4GHz antenna to the ESP32 module's IPEX connector for reliable Wi-Fi and Bluetooth signal reception. | 1 | $10.23 | $10.23 | [Aliexpress](https://www.aliexpress.com/item/1005012839987115.html?spm=a2g0o.productlist.main.17.22487e4aAc5l2Y&algo_pvid=5c910e90-451d-4eb2-b6c2-a9b67833a777&algo_exp_id=5c910e90-451d-4eb2-b6c2-a9b67833a777-16&pdp_ext_f=%7B%22order%22%3A%228%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%2112.90%215.00%21%21%2159.41%2123.03%21%402101e80b17915677645397405e0de8%2112000059523438374%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000216666461&curPageLogUid=c4JAZG0wstCM&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005012839987115%7C_p_origin_prod%3A) |
| [Mini Optical Audio Adapter 3.5mm Female Jack to Digital Toslink Male Plug For Amplifier Optical Audio Cable](https://www.aliexpress.com/item/1005009093776595.html?spm=a2g0o.productlist.main.6.18e47348zV59Cg&algo_pvid=f7d36bc0-4bf3-4565-acc6-9d3dda11366b&algo_exp_id=f7d36bc0-4bf3-4565-acc6-9d3dda11366b-5&pdp_ext_f=%7B%22order%22%3A%22344%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%213.75%213.75%21%21%2117.28%2117.28%21%402101de2517917626141065991e144b%2112000047887714520%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895&curPageLogUid=tx4CSSuvm17a&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005009093776595%7C_p_origin_prod%3A) | That adapter is used to convert a standard 3.5mm audio cable connection into a digital optical Toslink plug. | 1 | $2.97 | $2.97 | [Aliexpress](https://www.aliexpress.com/item/1005009093776595.html?spm=a2g0o.productlist.main.6.18e47348zV59Cg&algo_pvid=f7d36bc0-4bf3-4565-acc6-9d3dda11366b&algo_exp_id=f7d36bc0-4bf3-4565-acc6-9d3dda11366b-5&pdp_ext_f=%7B%22order%22%3A%22344%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%213.75%213.75%21%21%2117.28%2117.28%21%402101de2517917626141065991e144b%2112000047887714520%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895&curPageLogUid=tx4CSSuvm17a&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005009093776595%7C_p_origin_prod%3A) |
| [SJ-3523-SMT-TR](https://jlcpcb.com/partdetail/CUI-SJ_3523_SMTTR/C4991872) | The SJ-3523-SMT-TR is a surface-mount 3.5mm stereo audio jack used here to plug in your audio cable and bring the analog sound signals onto the PCB. | 1 | $1.42 | $1.42 | [JLCPCB](https://jlcpcb.com/partdetail/CUI-SJ_3523_SMTTR/C4991872) |
| [PCM1808PWR](https://jlcpcb.com/partdetail/TexasInstruments-PCM1808PWR/C55513) | The PCM1808PWR is a 24-bit stereo analog-to-digital converter (ADC) used here to convert your analog audio signals into digital I2S data for the ESP32. | 1 | $0.79 | $0.79 | [JLCPCB](https://jlcpcb.com/partdetail/TexasInstruments-PCM1808PWR/C55513) |
| [BC817](https://jlcpcb.com/partdetail/2494-BC817_RANGE_100_250/C2137) | The BC817 is a general-purpose NPN transistor used here to act as a switch—typically for driving indicator LEDs, controlling power lines, or handling USB reset signals in your circuit. | 2 | $0.07 | $0.14 | [JLCPCB](https://jlcpcb.com/partdetail/2494-BC817_RANGE_100_250/C2137) |
| [Resistor Kit](https://www.aliexpress.com/item/1005011772534173.html?spm=a2g0o.productlist.main.1.32f07cfcDAKt5h&algo_pvid=bd1a798f-1e1b-462e-8c2c-269d4ca4ad23&algo_exp_id=bd1a798f-1e1b-462e-8c2c-269d4ca4ad23-0&pdp_ext_f=%7B%22order%22%3A%229084%22%2C%22spu_best_type%22%3A%22price%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%216.03%214.58%21%21%2127.77%2121.09%21%402101e7a317917668551207826e0eb3%2112000056500803485%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000218598348&curPageLogUid=XejYrzptMOfz&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005011772534173%7C_p_origin_prod%3A) | A resistor kit is a collection of various resistors with different resistance values used across your circuit for setting up pull-up/pull-down lines, limiting current to LEDs, and biasing transistors. | 1 | $4.78 | $4.78 | [Aliexpress](https://www.aliexpress.com/item/1005011772534173.html?spm=a2g0o.productlist.main.1.32f07cfcDAKt5h&algo_pvid=bd1a798f-1e1b-462e-8c2c-269d4ca4ad23&algo_exp_id=bd1a798f-1e1b-462e-8c2c-269d4ca4ad23-0&pdp_ext_f=%7B%22order%22%3A%229084%22%2C%22spu_best_type%22%3A%22price%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21CAD%216.03%214.58%21%21%2127.77%2121.09%21%402101e7a317917668551207826e0eb3%2112000056500803485%21sea%21CA%218048449113%21X%211%210%21n_tag%3A-29911%3Bd%3A979b4513%3Bm03_new_user%3A-29895%3BpisId%3A5000000218598348&curPageLogUid=XejYrzptMOfz&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005011772534173%7C_p_origin_prod%3A) |
| [PCB Manufacturing](https://cart.jlcpcb.com/quote?stencilLayer=2&stencilWidth=100&stencilLength=100&stencilCounts=5&plateType=1&spm=Jlcpcb.Homepage.1010) | This will be the board where I will connect all my parts too | 1 | $8.53 | $8.53 | [JLCPCB](https://cart.jlcpcb.com/quote?stencilLayer=2&stencilWidth=100&stencilLength=100&stencilCounts=5&plateType=1&spm=Jlcpcb.Homepage.1010) |
| **Parts subtotal** | — | — | — | **$52.25** | — |
| **Tax & shipping** | — | — | — | **$12.00** | — |
| **Total** | — | — | — | **$64.25** | — |


---

## License

[License, e.g. CERN-OHL-P or MIT]. See [LICENSE.md](https://github.com/Shashvat1333/Bluetooth-Audio-Transmiter/blob/main/LICENCE.md).
