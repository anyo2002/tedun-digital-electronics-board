# System Interface Design

## Overview

During the development of the TEDUN project, an **LctechPi board (F133A-based)** was used as a validation platform to test and define the communication interface between the processor and the FPGA.

This approach allowed:
- Early validation of hardware communication
- Definition of pin connections before PCB design
- Testing of FPGA programming workflows

---

## 🧪 Development Platform

The LctechPi board was used to:

- Configure and run **Tina Linux**
- Install required system packages
- Validate communication interfaces

A WiFi module was also tested successfully; however, it was **not included in the final design** due to:
- Lack of necessity for FPGA programming
- Additional hardware cost

---

## 🔌 Communication Architecture

The main role of the system interface is to enable communication between:

- **Computer ↔ F133A processor**
- **F133A processor ↔ FPGA (Trion T8)**

### Data Flow

1. The computer sends data via **UART (RX/TX pins)**
2. The F133A receives and processes the data
3. The processor forwards configuration data to the FPGA via:
   - **JTAG (primary method)**
   - Optional interfaces (SPI, UART)

This architecture allows the processor to act as an **intermediary controller** for FPGA configuration and communication.

---

## 🛠️ FPGA Programming Tool Selection

To program the FPGA, an open-source tool was required with the following characteristics:

- Compatibility with **Efinix Trion T8 FPGA**
- Support for **JTAG programming**
- Ability to run on an embedded Linux system (F133A)

### Evaluated Options

#### ❌ openFPGALoader
- Supports Trion FPGA family
- Primarily designed for **memory-based programming (Flash)**

Limitation:
- Does not support the required **JTAG-based programming via processor GPIO**

➡️ **Rejected for this project**

---

#### ✅ OpenOCD
- Open-source hardware debugging tool
- Supports:
  - Multiple JTAG interfaces
  - FPGA programming
  - Embedded Linux environments

Advantages:
- Compatible with **Trion T8 FPGA**
- Supports **GPIO-based JTAG implementation**
- Easily integrable with the F133A system

➡️ **Selected as the final solution**

---

## 🎯 Design Outcome

This interface design enables:

- Reliable communication between processor and FPGA
- FPGA programming directly from the embedded system
- Flexible experimentation with hardware modules
- A fully self-contained development board architecture

---

## 🚀 Key Takeaways

- Early prototyping with development boards reduces hardware risks
- Open-source tools provide flexibility and integration advantages
- JTAG over GPIO is a powerful method for embedded FPGA programming