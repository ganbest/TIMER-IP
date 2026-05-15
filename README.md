# TIMER-IP — Timer Management IP Core (APB Interface)

> A fully synthesizable, APB-compliant Timer IP Core designed for SoC integration.  
> Includes RTL schematics, functional verification, simulation waveforms, and full test coverage evidence.

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Repository Structure](#2-repository-structure)
3. [Top-Level Architecture](#3-top-level-architecture)
4. [Module Descriptions](#4-module-descriptions)
   - [APB Interface](#41-apb-interface)
   - [Counter Module](#42-counter-module)
   - [Counter Control Module](#43-counter-control-module)
   - [Register Module](#44-register-module)
5. [RTL Design Details](#5-rtl-design-details)
   - [APB RTL](#51-apb-rtl)
   - [Counter RTL](#52-counter-rtl)
   - [Control RTL](#53-control-rtl)
   - [Register RTL](#54-register-rtl)
   - [Interrupt RTL](#55-interrupt-rtl)
   - [Top-Level RTL](#56-top-level-rtl)
6. [Test Plan](#6-test-plan)
7. [Simulation Results — Testcase](#7-simulation-results--testcase)
8. [Verification Results — Ket Qua](#8-verification-results--ket-qua)
9. [Design Specifications](#9-design-specifications)

---

## 1. Project Overview

**TIMER-IP** is a digital hardware IP core implementing a configurable timer with an **APB (Advanced Peripheral Bus / AMBA 2.0)** slave interface. The design is modular, fully synthesizable in Verilog/VHDL, and verified with a complete testbench suite.

Key capabilities:
- Configurable up/down counting with preset load
- Interrupt generation on overflow, underflow, and compare-match events
- APB-compliant register map for software-driven configuration
- Complete RTL-to-verification documentation

---

## 2. Repository Structure

```
TIMER-IP/
├── README.md                        # This document
├── Testplan (1).xlsx                # Full test plan & coverage tracking
│
├── Top_Module.png                   # Top-level block diagram
├── Counter_Module.png               # Counter block diagram
├── Counter_control_module.png       # Counter control block diagram
├── Register_Module_1.png            # Register block diagram (part 1)
├── Register_Module_2.png            # Register block diagram (part 2)
├── Register_Module_3.png            # Register block diagram (part 3)
│
├── rtl/                             # RTL schematic screenshots (14 images)
│   ├── apb.png                      # APB interface RTL view (1)
│   ├── apb2.png                     # APB interface RTL view (2)
│   ├── counter1.png                 # Counter RTL view (1)
│   ├── counter2.png                 # Counter RTL view (2)
│   ├── ctrl1.png                    # Control logic RTL view (1)
│   ├── ctrl2.png                    # Control logic RTL view (2)
│   ├── interrupt.png                # Interrupt logic RTL view
│   ├── register1.png                # Register RTL view (1)
│   ├── register2.png                # Register RTL view (2)
│   ├── register3.png                # Register RTL view (3)
│   ├── register4.png                # Register RTL view (4)
│   ├── top1.png                     # Top-level RTL view (1)
│   ├── top2.png                     # Top-level RTL view (2)
│   └── top3.png                     # Top-level RTL view (3)
│
├── testcase/                        # Simulation waveform screenshots (56 images)
│   └── Screenshot 2026-01-03 *.png
│
└── Ket qua/                         # Final verification results
    ├── Ket qua coverage.jpg         # Code coverage report
    ├── Screenshot 2025-12-03 190429.png
    ├── Screenshot 2025-12-03 190444.png
    ├── Screenshot 2025-12-03 190823.png
    └── Screenshot 2025-12-19 221253.png
```

---

## 3. Top-Level Architecture

The top-level module integrates all submodules under a single APB slave interface:

![Top-Level Block Diagram](Top_Module.png)

**Top-Level RTL Views:**

| View 1 | View 2 | View 3 |
|--------|--------|--------|
| ![Top RTL 1](rtl/top1.png) | ![Top RTL 2](rtl/top2.png) | ![Top RTL 3](rtl/top3.png) |

---

## 4. Module Descriptions

### 4.1 APB Interface

Implements the AMBA APB 2.0 slave protocol, translating bus transactions into internal register read/write operations.

**Signals:**

| Signal | Direction | Description |
|--------|-----------|-------------|
| PCLK | Input | Bus clock |
| PRESETn | Input | Active-low reset |
| PSEL | Input | Slave select |
| PENABLE | Input | Enable phase |
| PWRITE | Input | Write enable |
| PADDR | Input | Address bus |
| PWDATA | Input | Write data |
| PRDATA | Output | Read data |
| PREADY | Output | Transfer ready |

---

### 4.2 Counter Module

Configurable counter supporting up/down counting, preset load, and overflow/underflow detection.

![Counter Block Diagram](Counter_Module.png)

**Features:**
- Programmable count direction (up / down)
- Synchronous preset/load
- Overflow and underflow flags
- Configurable bit-width

---

### 4.3 Counter Control Module

State machine that governs timer operation modes, event handling, and interrupt triggering.

![Counter Control Block Diagram](Counter_control_module.png)

**Responsibilities:**
- Timer start / stop / pause control
- Mode transitions (one-shot, free-run, PWM)
- Compare-match detection
- Interrupt signal generation

---

### 4.4 Register Module

APB-accessible control and status registers for software configuration of the timer.

| View | Image |
|------|-------|
| Register Block (1) | ![Register 1](Register_Module_1.png) |
| Register Block (2) | ![Register 2](Register_Module_2.png) |
| Register Block (3) | ![Register 3](Register_Module_3.png) |

**Register Map (summary):**

| Offset | Name | Description |
|--------|------|-------------|
| 0x00 | CTRL | Timer control register |
| 0x04 | LOAD | Counter load value |
| 0x08 | COUNT | Current counter value (read-only) |
| 0x0C | CMP | Compare match value |
| 0x10 | INT_EN | Interrupt enable mask |
| 0x14 | INT_STATUS | Interrupt status / clear |

---

## 5. RTL Design Details

### 5.1 APB RTL

| APB View 1 | APB View 2 |
|------------|------------|
| ![APB RTL 1](rtl/apb.png) | ![APB RTL 2](rtl/apb2.png) |

---

### 5.2 Counter RTL

| Counter View 1 | Counter View 2 |
|----------------|----------------|
| ![Counter RTL 1](rtl/counter1.png) | ![Counter RTL 2](rtl/counter2.png) |

---

### 5.3 Control RTL

| Control View 1 | Control View 2 |
|----------------|----------------|
| ![Control RTL 1](rtl/ctrl1.png) | ![Control RTL 2](rtl/ctrl2.png) |

---

### 5.4 Register RTL

| View 1 | View 2 |
|--------|--------|
| ![Register RTL 1](rtl/register1.png) | ![Register RTL 2](rtl/register2.png) |

| View 3 | View 4 |
|--------|--------|
| ![Register RTL 3](rtl/register3.png) | ![Register RTL 4](rtl/register4.png) |

---

### 5.5 Interrupt RTL

![Interrupt Logic RTL](rtl/interrupt.png)

---

### 5.6 Top-Level RTL

| View 1 | View 2 | View 3 |
|--------|--------|--------|
| ![Top RTL 1](rtl/top1.png) | ![Top RTL 2](rtl/top2.png) | ![Top RTL 3](rtl/top3.png) |

---

## 6. Test Plan

The full test plan is documented in:

> **[Testplan (1).xlsx](Testplan%20(1).xlsx)**

Contents include:
- Test case IDs and descriptions
- Input stimulus definitions
- Expected output per test case
- Pass/Fail status
- Functional coverage metrics
- Code coverage targets

---

## 7. Simulation Results — Testcase

The `testcase/` directory contains **56 simulation waveform screenshots** captured during functional verification on 2026-01-03. Each screenshot corresponds to one or more test cases from the test plan.

**Group 1 — Basic Functionality (08:51 – 08:58)**

| # | Screenshot |
|---|------------|
| 1 | ![TC01](testcase/Screenshot%202026-01-03%20085152.png) |
| 2 | ![TC02](testcase/Screenshot%202026-01-03%20085210.png) |
| 3 | ![TC03](testcase/Screenshot%202026-01-03%20085229.png) |
| 4 | ![TC04](testcase/Screenshot%202026-01-03%20085243.png) |
| 5 | ![TC05](testcase/Screenshot%202026-01-03%20085251.png) |
| 6 | ![TC06](testcase/Screenshot%202026-01-03%20085355.png) |
| 7 | ![TC07](testcase/Screenshot%202026-01-03%20085405.png) |
| 8 | ![TC08](testcase/Screenshot%202026-01-03%20085414.png) |
| 9 | ![TC09](testcase/Screenshot%202026-01-03%20085431.png) |
| 10 | ![TC10](testcase/Screenshot%202026-01-03%20085444.png) |
| 11 | ![TC11](testcase/Screenshot%202026-01-03%20085454.png) |
| 12 | ![TC12](testcase/Screenshot%202026-01-03%20085505.png) |
| 13 | ![TC13](testcase/Screenshot%202026-01-03%20085520.png) |
| 14 | ![TC14](testcase/Screenshot%202026-01-03%20085530.png) |
| 15 | ![TC15](testcase/Screenshot%202026-01-03%20085548.png) |

**Group 2 — Counter & Control Verification (08:57 – 09:01)**

| # | Screenshot |
|---|------------|
| 16 | ![TC16](testcase/Screenshot%202026-01-03%20085701.png) |
| 17 | ![TC17](testcase/Screenshot%202026-01-03%20085712.png) |
| 18 | ![TC18](testcase/Screenshot%202026-01-03%20085726.png) |
| 19 | ![TC19](testcase/Screenshot%202026-01-03%20085753.png) |
| 20 | ![TC20](testcase/Screenshot%202026-01-03%20090020.png) |
| 21 | ![TC21](testcase/Screenshot%202026-01-03%20090030.png) |
| 22 | ![TC22](testcase/Screenshot%202026-01-03%20090039.png) |
| 23 | ![TC23](testcase/Screenshot%202026-01-03%20090057.png) |
| 24 | ![TC24](testcase/Screenshot%202026-01-03%20090115.png) |
| 25 | ![TC25](testcase/Screenshot%202026-01-03%20090129.png) |

**Group 3 — Register & Interrupt Verification (09:03 – 09:07)**

| # | Screenshot |
|---|------------|
| 26 | ![TC26](testcase/Screenshot%202026-01-03%20090316.png) |
| 27 | ![TC27](testcase/Screenshot%202026-01-03%20090323.png) |
| 28 | ![TC28](testcase/Screenshot%202026-01-03%20090338.png) |
| 29 | ![TC29](testcase/Screenshot%202026-01-03%20090357.png) |
| 30 | ![TC30](testcase/Screenshot%202026-01-03%20090405.png) |
| 31 | ![TC31](testcase/Screenshot%202026-01-03%20090412.png) |
| 32 | ![TC32](testcase/Screenshot%202026-01-03%20090419.png) |
| 33 | ![TC33](testcase/Screenshot%202026-01-03%20090643.png) |
| 34 | ![TC34](testcase/Screenshot%202026-01-03%20090658.png) |

**Group 4 — APB & Integration Tests (09:12 – 09:17)**

| # | Screenshot |
|---|------------|
| 35 | ![TC35](testcase/Screenshot%202026-01-03%20091231.png) |
| 36 | ![TC36](testcase/Screenshot%202026-01-03%20091239.png) |
| 37 | ![TC37](testcase/Screenshot%202026-01-03%20091256.png) |
| 38 | ![TC38](testcase/Screenshot%202026-01-03%20091314.png) |
| 39 | ![TC39](testcase/Screenshot%202026-01-03%20091322.png) |
| 40 | ![TC40](testcase/Screenshot%202026-01-03%20091333.png) |
| 41 | ![TC41](testcase/Screenshot%202026-01-03%20091341.png) |
| 42 | ![TC42](testcase/Screenshot%202026-01-03%20091346.png) |
| 43 | ![TC43](testcase/Screenshot%202026-01-03%20091355.png) |
| 44 | ![TC44](testcase/Screenshot%202026-01-03%20091407.png) |
| 45 | ![TC45](testcase/Screenshot%202026-01-03%20091420.png) |
| 46 | ![TC46](testcase/Screenshot%202026-01-03%20091429.png) |
| 47 | ![TC47](testcase/Screenshot%202026-01-03%20091437.png) |
| 48 | ![TC48](testcase/Screenshot%202026-01-03%20091522.png) |
| 49 | ![TC49](testcase/Screenshot%202026-01-03%20091605.png) |
| 50 | ![TC50](testcase/Screenshot%202026-01-03%20091710.png) |
| 51 | ![TC51](testcase/Screenshot%202026-01-03%20091719.png) |
| 52 | ![TC52](testcase/Screenshot%202026-01-03%20091727.png) |
| 53 | ![TC53](testcase/Screenshot%202026-01-03%20091741.png) |
| 54 | ![TC54](testcase/Screenshot%202026-01-03%20091802.png) |
| 55 | ![TC55](testcase/Screenshot%202026-01-03%20091811.png) |
| 56 | ![TC56](testcase/Screenshot%202026-01-03%20091817.png) |

---

## 8. Verification Results — Ket Qua

Final verification evidence from the `Ket qua/` directory:

### Code Coverage Report

![Coverage Report](Ket%20qua/Ket%20qua%20coverage.jpg)

### Simulation Output — December 2025

| Screenshot | Description |
|------------|-------------|
| ![Result 1](Ket%20qua/Screenshot%202025-12-03%20190429.png) | Simulation output (Dec 3, 19:04) |
| ![Result 2](Ket%20qua/Screenshot%202025-12-03%20190444.png) | Simulation output (Dec 3, 19:04) |
| ![Result 3](Ket%20qua/Screenshot%202025-12-03%20190823.png) | Simulation output (Dec 3, 19:08) |
| ![Result 4](Ket%20qua/Screenshot%202025-12-19%20221253.png) | Final verification run (Dec 19, 22:12) |

---

## 9. Design Specifications

| Parameter | Value |
|-----------|-------|
| Bus Protocol | APB (AMBA 2.0) |
| Design Language | Verilog / VHDL |
| Architecture | Modular, fully synthesizable |
| Interrupt Support | Yes (overflow, underflow, compare-match) |
| Timer Modes | One-shot, Free-run, PWM |
| Verification | Full functional + coverage |
| RTL Views | 14 schematic screenshots |
| Test Cases | 56 simulation screenshots |
| Coverage Evidence | Code coverage report included |
| Status | Complete |

---

*TIMER-IP — Verified and documented. All RTL views, simulation waveforms, and coverage results are included in this repository.*
