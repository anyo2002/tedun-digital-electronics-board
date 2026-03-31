# TEDUN - Digital Electronics Development Board

## 📌 Overview
The **TEDUN (Tarjeta de Electrónica Digital de la UNAL)** is an academic project developed as part of an Embedded Systems course in Electronic Engineering.

> ⚠️ Note: The project name does not represent an official product of UNAL. It only reflects the academic context in which it was developed.

This project consists of the design and implementation of a **custom development board** based on:
- A **F133A RISC-V processor**
- A **Trion T8Q144C3 FPGA**

The board enables hardware experimentation with multiple modules (Bluetooth, audio, infrared, etc.) using communication protocols such as **UART, I2S, SPI**, and **GPIO**.

---

## 🎯 Objectives
- Design a complete embedded development board
- Enable communication between a processor and an FPGA
- Support hardware testing using multiple digital interfaces
- Develop schematics and PCB using KiCad
- Implement FPGA programming using open-source tools

---

## ⚙️ Key Features

- Powered via **Micro-USB (~5V)**
- Boot via **SD card with Tina Linux**
- UART communication with a computer (via USB-UART converter)
- FPGA programming using **OpenOCD (JTAG)**
- Communication between F133A and FPGA via **UART or SPI**
- 32 GPIO pins organized in **4 banks (3.3V + 5V output)**

---

## 🏗️ System Architecture

The system integrates:
- F133A processor (RISC-V)
- Trion T8 FPGA
- SD card storage
- Multiple communication interfaces

### Interfaces

#### 🔹 JTAG
Used to program the FPGA through OpenOCD running on the F133A.

#### 🔹 SD Card
Stores:
- Tina Linux OS
- Boot partitions
- Drivers and applications
- FPGA configuration files

#### 🔹 UART & I2S
- UART for communication with PC and FPGA
- I2S for audio/data transmission

#### 🔹 GPIO
- 32 configurable pins
- Used for hardware testing and module integration

---

## 🧩 Block Diagram

![Block Diagram](images/Block_diagram.jpg)

---

## 🔌 Interface Design

To validate the system, an **LctechPi board (F133A-based)** was used for testing.

Key points:
- Tina Linux was configured and deployed
- Required system packages were installed
- WiFi was tested but not included (cost optimization)

### Tool Selection

Two tools were evaluated:

#### ❌ openFPGALoader
- Supports Trion FPGA
- Limited to memory-based programming (Flash)
- ❌ Not suitable for JTAG-based programming via F133A

#### ✅ OpenOCD
- Open-source
- Supports JTAG
- Compatible with Trion FPGA
- ✔️ Selected solution

---

## 🧪 FPGA Programming with OpenOCD

OpenOCD was compiled for RISC-V and deployed on the board.

### Setup Steps:
1. Compile OpenOCD from source:
   https://github.com/riscv/riscv-openocd

2. Copy binary to:
/bin/

3. Copy scripts to:
/usr/local/share/openocd/scripts

4. Create configuration file:
openocd.cfg

### Example Configuration (JTAG via GPIO)

```bash
adapter driver sysfsgpio
transport select jtag
sysfsgpio jtag_nums 2 3 1 4

set CHIPNAME fpga_trion
source [find fpga/efinix_trion.cfg]

init
pld load fpga_trion.pld GPIO_demo.bit
```

## 🛠️ Hardware Design

The hardware design is based on:

- MangoPi (F133A reference)

![Power Supply](images/F133A.png)

- LctechPi board

- Trion T8 FPGA reference boards

## 🔋 Power Supply

![RY1303](images/RY1303.png)

- Input: Micro-USB (5V)
- Regulation using:
  - AMS1117 (instead of RY1303 from the MangoPi board)
  - Multiple LDO regulators

## Voltage Levels:
- 3.3V → I/O, GPIO, oscillator

![FAN2558S33X](images/LDO_FAN2558S33X.png)

- 1.8V → DRAM

![FAN2558S18X](images/LDO_FAN2558S18X.png)

- 2.8V → Communication block (PE)

![TPS78228DDCR](images/TPS78228DDCR.png)

- 1.2V → FPGA core

![TPS73101DBVR](images/TPS73101DBVR.png)

- 0.9V → Processor core

![AP7343Q-09W5-7](images/LDO_AP7343Q-09W5-7.png)

## ⚡ Power Sequencing

Implemented using:
- LM3880 power sequencer

![LM3880](images/LM3880_power.png)

Sequence: 
1. Enable FPGA + DRAM + PE
2. Enable processor core (0.9V)
3. Enable FPGA I/O

## 💾 SD Card
- Used for booting Tina Linux
- Based on MangoPi reference design

![SD F133](images/SD_F133.png)

- Direct connection to F133A

![SD Module](images/SD.png)

## 🧠 F133A Connections

![F133 Connections](images/F133_connections.png)

- JTAG → PG6–PG9
- UART0 → PE2 / PE3
- UART → PC6 / PC7
- SD → PF0–PF6
- SPI → PC2–PC7

## 🔲 FPGA (Trion T8)

![PCB layout](images/FPGA_T8Q144C3.png)

- 13 banks available
- Selected banks: 1A, 3A, 3BC, 3D
- Supports:
  - JTAG programming
  - SPI & UART communication
  - GPIO expansion

## 🔌 GPIO

![GPIO](images/GPIO.png)

- 32 GPIO pins (4 banks × 8 pins)
- Each bank includes:
  - 3.3V output
  - GND
- Additional:
  - 5V output
  - UART interface
## ⏱️ Oscillators
- FPGA: 25 MHz oscillator

![Oscillator 25MHz](images/Osc_25MHz.png)

- Processor: MangoPi reference oscillator

![Oscillator F133](images/Osc_F133.png)

## 🧾 PCB Design

![PCB layout](images/PCB_layout.png)

Designed in KiCad:
- Compact layout
- Optimized routing
- 3D visualization available

![Top 3D](images/Top_3D.png)

![Bottom 3D](images/Bottom_3D.png)

## 📍 Pinout
![Pinout](images/Pinout.png)

![Pin function](images/Pin_function.png)

- 32 GPIO pins available
- Organized in 4 banks
- Flexible usage:
  - GPIO
  - Communication interfaces
  - Special FPGA functions

## 📚 References
- F133A Datasheet
- Trion T8 Datasheet
- MangoPi Schematics

## 🧰 Tools & Technologies
- KiCad (PCB design)
- OpenOCD (FPGA programming)
- Tina Linux
- RISC-V architecture
- Embedded Linux

## 🚀 Skills Demonstrated
- Embedded Systems Design
- FPGA Integration
- Hardware Architecture
- PCB Design (KiCad)
- Power Electronics (LDO, sequencing)
- Communication Protocols (UART, SPI, I2S, JTAG)

## 👤 Authors

- Angel RUIZ
- Santiago DIAZ
- Daniel CELY
