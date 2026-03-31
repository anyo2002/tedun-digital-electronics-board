# Architecture

## Overview
The TEDUN board integrates a RISC-V processor (F133A) with a Trion T8 FPGA.

## Interfaces

### JTAG
Used for FPGA programming via OpenOCD.

### SD Card
Used for booting Tina Linux and storing FPGA bitstreams.

### UART & I2S
Used for communication between:
- PC ↔ F133A
- F133A ↔ FPGA

### GPIO
32 configurable pins used for hardware experimentation.