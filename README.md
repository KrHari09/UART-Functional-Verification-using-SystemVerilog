<div align="center">

# UART Functional Verification using SystemVerilog

![Language](https://img.shields.io/badge/Language-SystemVerilog%20IEEE%201800--2017-blue?style=for-the-badge&logo=v)
![Tool](https://img.shields.io/badge/Tool-ModelSim%20%7C%20QuestaSim-orange?style=for-the-badge)
![Simulator](https://img.shields.io/badge/Simulator-Icarus%20Verilog-yellow?style=for-the-badge)
![Type](https://img.shields.io/badge/Type-Functional%20Verification-red?style=for-the-badge)
![Methodology](https://img.shields.io/badge/Methodology-Constrained%20Random-purple?style=for-the-badge)
![Coverage](https://img.shields.io/badge/Functional%20Coverage-92.92%25-brightgreen?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-All%20Tests%20Passed-brightgreen?style=for-the-badge&logo=checkmarx)

</div>


## Table of Contents

1. [Introduction](#1-introduction)
2. [UART Protocol Background](#2-uart-protocol-background)
3. [Verification Objectives](#3-verification-objectives)
4. [Testbench Architecture](#4-testbench-architecture)
5. [RTL Design Under Test (DUT)](#5-rtl-design-under-test-dut)
6. [Constrained Random Testing](#6-constrained-random-testing)
7. [SystemVerilog Assertions (SVA)](#7-systemverilog-assertions-sva)
8. [Functional Coverage](#8-functional-coverage)
9. [Waveform Debugging](#9-waveform-debugging)
10. [Project File Structure](#10-project-file-structure)
11. [How to Run](#11-how-to-run)
12. [Results](#12-results)
13. [Conclusion](#13-conclusion)
14. [References](#14-references)

---

## 1. Introduction

The **Universal Asynchronous Receiver/Transmitter (UART)** is one of the most widely used serial communication protocols in embedded systems, SoCs, and FPGA-based designs. Ensuring its correct functional behaviour under varied conditions — different baud rates, parity settings, back-to-back frames, and error injection — requires a rigorous, structured verification approach.

This project implements a **complete SystemVerilog functional verification environment** for a parameterized UART module. The testbench applies industry-standard techniques including:

- **Constrained Random Verification (CRV)** — automated generation of valid and boundary-condition test stimuli
- **SystemVerilog Assertions (SVA)** — protocol-level property checking bound directly to DUT signals
- **Functional Coverage** — `covergroup`/`coverpoint` tracking to measure test completeness
- **Self-checking Scoreboard** — automatic pass/fail comparison of transmitted vs. received data
- **Waveform Debugging** — VCD dump for GTKWave / ModelSim waveform analysis

The goal is to achieve **≥ 90% functional coverage** across all defined scenarios and demonstrate zero mismatches between transmitted and received UART frames in a loopback configuration.

---


## 2. UART Protocol Background

UART is a serial, **asynchronous**, full-duplex communication protocol. "Asynchronous" means there is no shared clock between transmitter and receiver — both ends must be pre-configured to use the **same baud rate**.

### 2.1 Frame Structure

A UART data frame consists of the following fields transmitted serially, LSB first:
<img width="1440" height="466" alt="image" src="https://github.com/user-attachments/assets/8a980cde-d4ce-475d-89b9-0f1e7d67d94a" />


| Field | Value | Duration | Description |
|---|---|---|---|
| **Idle** | HIGH (1) | Until transmission | Line rests high |
| **Start Bit** | LOW (0) | 1 bit period | Signals start of frame |
| **Data Bits** | D0–D7 | 8 bit periods | LSB first |
| **Parity Bit** | Even/Odd | 1 bit period (optional) | Error detection |
| **Stop Bit** | HIGH (1) | 1 bit period | Signals end of frame |

### 2.2 Baud Rate

Baud rate defines the number of bits transmitted per second. Both transmitter and receiver must match. Common values:

| Baud Rate | Bit Period | Typical Use |
|---|---|---|
| 9,600 | 104.2 µs | Legacy / low-speed |
| 115,200 | 8.68 µs | Most common |
| 230,400 | 4.34 µs | High speed |


### 2.3 Timing Diagram
<img width="1440" height="580" alt="image" src="https://github.com/user-attachments/assets/22693d4d-ac51-48cf-bbd1-76012da6cc7d" />



> *Each bit is held stable for exactly one baud period (e.g., 8.68 µs at 115200 baud).*

### 2.4 Parity

- **Even parity:** parity bit set so total number of 1s in data + parity = even
- **Odd parity:** parity bit set so total number of 1s = odd
- If the received parity doesn't match, a **parity error** is flagged

---

## 3. Verification Objectives

| # | Objective | Technique Used |
|---|---|---|
| 1 | Verify correct serialization of all 256 data patterns | Constrained Random + Directed |
| 2 | Verify baud rate operation at 9600, 115200, 230400 | CRV with baud constraint |
| 3 | Verify parity generation (even/odd) and error detection | CRV + SVA assertions |
| 4 | Verify start bit LOW, stop bit HIGH at all times | SVA property checks |
| 5 | Verify TX line held HIGH during idle | SVA |
| 6 | Verify back-to-back frame transmission | Zero idle_cycles constraint |
| 7 | Verify framing error detection on corrupted stop bit | Fault injection |
| 8 | Achieve ≥ 90% functional coverage | Covergroups |
| 9 | Zero scoreboard mismatches on 200+ transactions | Self-checking SB |

---

## 4. Testbench Architecture

The testbench follows a **layered, component-based architecture** modelled after industry-standard OOP verification methodology.

<img width="1440" height="1080" alt="image" src="https://github.com/user-attachments/assets/bfb13edb-bb0d-4412-862b-88e51f9bc2a7" />




