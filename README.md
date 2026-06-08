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
---

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
