# OpenOCD Configuration

## Overview

To configure and program the FPGA, **OpenOCD (Open On-Chip Debugger)** was used as the main tool.

Initial validation was performed using a *Nexys A7* board, where it was verified that the **sysfsgpio interface** combined with the **JTAG protocol** can successfully upload a `.bit` file to an FPGA.

---

## 🔧 Building OpenOCD for RISC-V

The first step is to compile OpenOCD with **RISC-V support** from source:

Repository:
https://github.com/riscv/riscv-openocd

The compilation is performed using the provided **cross-compilation scripts**, where the toolchain path must be configured.

This toolchain is obtained when generating the **Tina Linux image**:
https://github.com/mangopi-sbc/Tina-Linux

---

## 📦 Installation on the Board

After compilation:

- The OpenOCD binary is copied to:
/bin/

- The configuration scripts (`tcl` folder) are copied to:
/usr/local/share/openocd/scripts

This process is equivalent to installing OpenOCD on a standard Linux system.

---

## ⚙️ Configuration File

A configuration file named:
openocd.cfg

must be created in the scripts directory.

This file is executed every time the `openocd` command is run from the development board terminal.

---

## 🔌 JTAG Configuration via GPIO

To configure JTAG using the **GPIO pins of the F133A**, the following setup was tested and validated:

### Example (Xilinx FPGA)

```bash
adapter driver sysfsgpio
transport select jtag

# GPIO mapping: tck tms tdi tdo
sysfsgpio jtag_nums 2 3 1 4

tcl_port disabled
telnet_port disabled

set CHIPNAME fpga_xilinx
source [find cpld/xilinx-xc7.cfg]

init
pld load fpga_xilinx.pld GPIO_demo.bit
```
## 🔄 Configuration for Trion T8 FPGA
To use the Efinix Trion T8 FPGA, the configuration must be modified as follows:

```bash
sysfsgpio jtag_nums 2 3 1 4

set CHIPNAME fpga_trion
source [find fpga/efinix_trion.cfg]

pld load fpga_trion.pld GPIO_demo.bit
```
## ⚠️ Design Considerations
Although OpenOCD allows flexible GPIO configuration for JTAG, this is not recommended in the final TEDUN board, since:
1. JTAG pins are already hardwired in the PCB
2. Manual reconfiguration could introduce inconsistencies

## 📁 Bitstream Loading
The FPGA configuration file must be a .bit file.

This file is generated using the Efinix development software (Efinity):
https://www.efinixinc.com/products-efinity.html

It can be replaced dynamically depending on the desired hardware functionality.

## 📚 Additional Resources
- OpenOCD FPGA documentation:
https://openocd.org/doc/html/PLD_002fFPGA-Commands.html

- JTAG interface documentation:
https://openocd.org/doc/html/Debug-Adapter-Hardware.html

## 🚀 Summary

This implementation demonstrates:

- Successful integration of OpenOCD in an embedded Linux system
- FPGA programming via JTAG using GPIO
- Cross-compilation for RISC-V architecture
- Compatibility with Efinix Trion FPGA devices