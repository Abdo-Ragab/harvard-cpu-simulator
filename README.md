# Harvard CPU Simulator

A processor simulator written in C that models a 3-stage pipelined Harvard architecture CPU with custom instruction decoding, hazard forwarding, branching, and memory management.

Developed for CSEN601 – Computer Systems Architecture.

---

## Features

- 3-stage instruction pipeline (IF → ID → EX)
- Harvard architecture design
- 16-bit instruction encoding
- 64 general-purpose 8-bit registers
- Custom ISA with 12 instructions
- RAW hazard detection and forwarding
- Control hazard handling with pipeline flushing
- Separate instruction and data memory
- Status register (SREG) flag handling
- Assembly program parsing and execution
- Full cycle-by-cycle simulation logging
- Modular design with separate components for memory, registers, decoding, ALU, and pipeline control

---

## Architecture Overview

| Component | Specification |
|---|---|
| Architecture | Harvard |
| Pipeline Depth | 3 Stages |
| Instruction Memory | 1024 × 16-bit |
| Data Memory | 2048 × 8-bit |
| Registers | 64 × 8-bit GPRs |
| Status Register | 5 Flags (C, V, N, S, Z) |
| Instruction Size | 16-bit |
| ISA Size | 12 Instructions |

---

## Pipeline Design

The simulator implements a classic 3-stage pipeline:

```text
Instruction Fetch (IF)
        ↓
Instruction Decode (ID)
        ↓
Execute (EX)
```

The pipeline supports:
- Data hazard resolution using forwarding
- Control hazard handling using pipeline flushing
- Parallel execution of up to 3 instructions simultaneously

---

## Instruction Set

| Opcode | Mnemonic | Description |
|---|---|---|
| 0 | ADD | Addition |
| 1 | SUB | Subtraction |
| 2 | MUL | Multiplication |
| 3 | MOVI | Move Immediate |
| 4 | BEQZ | Branch if Equal Zero |
| 5 | ANDI | Bitwise AND Immediate |
| 6 | EOR | Bitwise XOR |
| 7 | BR | Unconditional Branch |
| 8 | SLC | Circular Left Shift |
| 9 | SRC | Circular Right Shift |
| 10 | LDR | Load from Memory |
| 11 | STR | Store to Memory |

---

## Project Structure

```text
harvard-cpu-simulator/
│
├── README.md
├── LICENSE
├── Makefile
├── .gitignore
│
├── src/
│   ├── main.c
│   ├── memory.c
│   ├── registers.c
│   ├── decoder.c
│   ├── alu.c
│   └── pipeline.c
│
├── include/
│   ├── memory.h
│   ├── registers.h
│   ├── decoder.h
│   ├── alu.h
│   └── pipeline.h
│
├── examples/
│   └── sample_program.txt
│
├── docs/
│   └── Project_Report.pdf
│
└── screenshots/
    └── pipeline_output.png
```

---

## Build Instructions

Compile the simulator using GCC:

```bash
gcc main.c memory.c registers.c decoder.c alu.c pipeline.c -o simulator
```

---

## Running the Simulator

Run the simulator with an assembly program file:

```bash
./simulator sample_program.txt
```

---

## Example Assembly Program

```assembly
MOVI R1 5
MOVI R2 10
ADD R1 R2
STR R1 20
LDR R3 20
```

---

## Example Output

```text
[IF] Fetched instruction at PC=0
[ID] Decoding instruction ADD R1 R2
[EX] Register Updated: R1 = 15
[ID] Forwarding applied to R1
```

---

## Hazard Handling

### Data Hazards (RAW)

The simulator resolves Read-After-Write (RAW) hazards using forwarding:

- Results computed in EX are forwarded directly to ID
- No pipeline stalls are required
- Forwarding paths are logged during execution

### Control Hazards

Branches and jumps are resolved in the EX stage using pipeline flushing:

- Incorrectly fetched instructions are invalidated
- Pipeline registers are cleared after a taken branch
- Fetch resumes from the correct target PC

---

## Status Register (SREG)

The simulator implements a 5-flag status register:

| Flag | Description |
|---|---|
| C | Carry |
| V | Overflow |
| N | Negative |
| S | Sign |
| Z | Zero |

Flags are updated dynamically based on ALU operations.

---

## Module Breakdown

| Module | Responsibility |
|---|---|
| `main.c` | Program entry point |
| `memory.c` | Instruction/data memory management |
| `registers.c` | Register file and SREG |
| `decoder.c` | Instruction decoding |
| `alu.c` | Arithmetic and logical execution |
| `pipeline.c` | Pipeline control and hazard handling |

---

## Simulation Features

The simulator provides detailed execution logs including:

- Per-cycle pipeline state
- Register updates
- Memory writes
- Hazard detection and forwarding
- Branch flush events
- Final register dump
- Final memory dump
- SREG state reporting

---

## Future Improvements

Potential future enhancements include:

- Cache simulation
- Branch prediction
- Multi-cycle execution units
- Out-of-order execution
- Pipeline visualization
- Interactive debugger
- Custom assembler frontend

---

## Documentation

A full technical report describing the architecture, implementation, and pipeline behavior is available in:

```text
/docs/Project_Report.pdf
```

---

## License

This project is licensed under the MIT License.

---

## Author

- Abdelrahman Ragab
- Mohamed Mahmoud
- Mohamed Fathi
- Omar Abdelwahab
- Badr Elmaghraby
- Abdelrahman Almozy
