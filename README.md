# UART Functional Verification using SystemVerilog

> **Academic Project** | Aug 2025 – Dec 2025  
> **Domain:** VLSI Design Verification | **Tool:** ModelSim / QuestaSim / Icarus Verilog  
> **Language:** SystemVerilog (IEEE 1800-2017)

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
