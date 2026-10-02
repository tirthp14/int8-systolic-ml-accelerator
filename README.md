# INT8 Systolic Array ML Accelerator

By: **Tirth Patel**

![RTL](https://img.shields.io/badge/RTL-SystemVerilog-orange)
![Verification](https://img.shields.io/badge/Verification-UVM-blue)
![Unit Tests](https://img.shields.io/badge/Unit%20Tests-cocotb-teal)
![Simulation](https://img.shields.io/badge/Simulation-Verilator-green)
![Bus](https://img.shields.io/badge/Bus-AXI4-yellow)
![Reference Model](https://img.shields.io/badge/Reference%20Model-NumPy-purple)
![CI](https://github.com/tirthp14/int8-systolic-ml-accelerator/actions/workflows/ci.yml/badge.svg)

A weight-stationary systolic array accelerator for INT8 matrix multiplication, implemented in SystemVerilog and modeled on TPU-style ML inference hardware. It includes an AXI4 DMA engine with ping-pong buffers, an AXI4-Lite register interface, and a UVM testbench checked against a NumPy golden model.

## Features

- 16×16 weight-stationary systolic array
- INT8 × INT8 multiply with 32-bit accumulation
- Tiling controller supporting arbitrary matrix sizes, including non-multiple-of-16 edge tiles
- Fixed-point requantization (multiplier + shift) back to INT8
- AXI4 DMA engine with ping-pong on-chip buffers to overlap transfers and compute
- AXI4-Lite control/status registers for host programming
- UVM testbench with constrained-random stimulus, AXI agents, RAL, and scoreboard
- SystemVerilog Assertions for AXI protocol checking and functional coverage
- cocotb unit tests for individual blocks

## Status

- [x] Repo structure
- [ ] CI pipeline (lint + tests on every push)
- [ ] Processing element (PE) + unit test
- [ ] 4×4 array with input skew / output deskew
- [ ] NumPy golden model (tiled matmul + requantization)
- [ ] 16×16 array
- [ ] Tiling controller FSM
- [ ] Requantization datapath
- [ ] Ping-pong buffers + AXI4 DMA engine
- [ ] AXI4-Lite CSR block
- [ ] UVM testbench (agents, RAL, scoreboard)
- [ ] SVA protocol checks + functional coverage
- [ ] Multi-seed regression
- [ ] Synthesis (area / timing)

## Running locally

**Requirements:** Verilator ≥ 5.0, Python 3, cocotb, NumPy

```bash
# Lint all RTL
make lint

# Run cocotb unit tests
make test

# Run golden-model tests
make model
```

---

## Repository Structure (Entirely a conceptual idea for now)
```
int8-systolic-ml-accelerator/
│
├── .github/
│   └── workflows/
│       └── ci.yml                       <- lint + tests on every push/PR
│
├── rtl/
│   ├── array/
│   │   ├── pe.sv                        <- processing element (MAC)
│   │   ├── systolic_array.sv
│   │   ├── skew.sv                      <- input skew / output deskew
│   │   └── ...
│   ├── ctrl/
│   │   ├── tile_ctrl.sv                 <- tiling controller FSM
│   │   └── requant.sv
│   ├── mem/
│   │   ├── pingpong_buf.sv
│   │   └── axi_dma.sv
│   ├── csr/
│   │   └── axil_csr.sv                  <- AXI4-Lite registers
│   ├── pkg/
│   │   └── accel_pkg.sv                 <- shared params and typedefs
│   └── top.sv
│
├── tb_cocotb/                           <- block-level unit tests
│   └── ...
│
├── uvm/                                 <- UVM testbench
│   ├── agents/
│   ├── env/
│   ├── tests/
│   └── ...
│
├── model/
│   └── golden.py                        <- NumPy reference model
│
├── synth/                               <- synthesis scripts and reports
│
├── docs/
│   ├── microarch.md
│   ├── verification_plan.md
│   └── bug_log.md
│
├── Makefile
└── README.md
```
