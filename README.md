# PicoRV Memory Over UART - CSE293 Final Project (Slides)

## 📌 Overview
This repository contains the **CSE293 Final Project slides** on implementing **memory access over UART** using the **PicoRV32** RISC-V CPU on an **iCE40UP5K FPGA**. The slides cover key aspects of FPGA implementation, pipeline optimizations, memory hierarchy, and firmware development.

## 📂 Repository Contents
- **Slides:** `CSE293_PicoRV_Final_Project.pdf`
- **Diagrams & Explanations** related to:
  - PicoRV32 CPU integration
  - UART communication setup
  - Memory optimizations & HPD cache usage
  - Firmware development using the RISC-V toolchain

## 🏗️ Key Topics Covered
### 1️⃣ **Hardware Setup**
- **FPGA:** iCE40UP5K
- **CPU:** PicoRV32 (4-stage pipeline, RISC-V RV32I)
- **Cache:** HPD L1 D-cache (multi-requester, out-of-order)
- **Memory Interface:**
  - 32-bit address and data width
  - Write-back policy
  - Transaction ID width: 4 bits (16 outstanding transactions)
  
### 2️⃣ **Software & Firmware Development**
- **UART Communication**
  - Baud rate: **115200**
  - **8 data bits, 1 start bit, 1 stop bit**
  - Uses `/dev/ttyACM0` (FTDI connection)
- **Firmware (C & Assembly)**
  - Built using **RISC-V GNU toolchain (2018 version recommended)**
  - ELF file generation (`program.elf`) for execution on FPGA
  - Implements low-level memory access

## 🚀 Features & Optimizations Explained in Slides
### ✅ **PicoRV32 CPU Integration**
- Uses the **test_ez** framework with modifications
- Memory: **128KB SRAM (14-bit physical address)**
- Configured for **FPGA execution on iCE40UP5K**

### ✅ **Pipeline Optimizations**
- **Miss Status Handling Registers (MSHRs)**:
  - **4 MSHR sets** (one per pipeline stage)
  - **2 MSHR ways** (allows handling two cache misses simultaneously)
- **Write Buffer**:
  - **8 directory entries**, **4 data entries**
  - Reduces memory stalls and improves performance

### ✅ **Cache System - HPD L1 D-Cache**
- Open-source, **multi-requester, out-of-order** cache
- Supports **uncacheable memory access**
- Implements **cacheability control** for segmented memory

## 📖 Understanding Memory Access Flow
### **🔄 Read/Write Data Over UART**
1. PC sends read/write command over UART
2. FPGA receives command and decodes it
3. **If cache hit** → Data is returned immediately
4. **If cache miss** → Fetch from main memory, store in cache, return data
5. Write-back caching ensures efficient memory updates

## 🧐 Key Learnings & Challenges
- **ELF Parsing:** Handling ELF headers and program sections for firmware execution.
- **SystemVerilog Optimization:** Structs, typedef macros, and DPI-C interfacing with C.
- **Verilator Debugging:** Fixing linting issues using `-Wno-fatal` and synthesis flags.
- **FPGA Timing Constraints:** Achieving a stable **18 MHz PLL clock** from 12 MHz input.

## 📌 References
- [PicoRV32 Documentation](https://github.com/cliffordwolf/picorv32)
- [HPD Cache Repo](https://github.com/openhwgroup/cv-hpdcache)
- [Verilator Guide](https://verilator.org/guide/latest/)

## 🔥 Authors
- **Aditya Bedekar**
- **Lennan Tuffy**
