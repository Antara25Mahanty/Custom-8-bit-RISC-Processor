# Custom-8-bit-RISC-Processor | VERILOG
Designed and implemented a custom 8-bit RISC processor in Verilog with a 13-bit ISA, datapath, control unit, 8x8 bit register file, instruction memory and simulation-based verification.

## Project overview
The objective of this project is to design a simple RISC-style processor from the ground up, starting from the instruction set architecture and progressing to individual RTL modules and their integration into a functional CPU.

The processor operates on 8-bit data and supports eight basic operations including arithmetic, logical, data movement, and shift operations. Instructions are stored in instruction memory and fetched sequentially using the program counter. Each instruction is decoded to generate the required register addresses, ALU operation, and control signals.

The complete processor data-path was implemented using synthesizable Verilog RTL and verified through simulation using EDA Playground and EPWave.

## Processor Specifications

| Parameter | Specification |
|---|---|
| Data width | 8 bits |
| Number of registers | 8 |
| Register width | 8 bits |
| Instruction width | 13 bits |
| Instruction memory | 256 × 13 bits = 416 Bytes |
| Register read ports | 2 |
| Register write ports | 1 |
| Opcode width | 3 bits |
| Addressing | Register-based |
| HDL | Verilog |
| Verification | EDA Playground / EPWave |


## Processor Architecture

The processor follows a simple fetch, decode, execute, and write-back datapath.

The instruction flow is:

PC → Instruction Memory → Instruction Decoder → Control Unit / Register File → ALU → Register File

The program counter provides the address of the current instruction. The instruction memory returns the corresponding 13-bit instruction, which is then decoded into the opcode, destination register, source registers, and shift direction.

The control unit translates the opcode into the ALU operation and generates the register-write and program-counter enable signals.

The register file provides two operands to the ALU through two independent read ports. The ALU performs the selected operation and returns the result to the destination register through the write port.


## Instruction Set Architecture

The processor uses a fixed 13-bit instruction format.

Instruction format:

| Field | Width |
|---|---:|
| Opcode | 3 bits |
| Destination Register (RD) | 3 bits |
| Source Register 1 (RS1) | 3 bits |
| Source Register 2 (RS2) | 3 bits |
| Direction (D) | 1 bit |
| Total | 13 bits |

The three-bit opcode provides eight possible operation codes.

### Instruction Set

| Opcode | Instruction | Operation |
|---|---|---|
| 000 | ADD | RD ← RS1 + RS2 |
| 001 | SUB | RD ← RS1 − RS2 |
| 010 | AND | RD ← RS1 & RS2 |
| 011 | OR | RD ← RS1 \| RS2 |
| 100 | XOR | RD ← RS1 ^ RS2 |
| 101 | MOV | RD ← RS1 |
| 110 | SHIFT | Shift RS1 according to D |
| 111 | SPECIAL | Reserved for future implementation |

For the SHIFT instruction:

- D = 0 → Left shift
- D = 1 → Right shift

## RTL Implementation

The processor is divided into independent RTL modules that are integrated at the CPU level.

### Program Counter

The program counter stores the address of the current instruction.
On reset, the program counter is initialized to zero. When the enable signal is asserted, the program counter increments to fetch the next instruction.

### Instruction Memory

The instruction memory stores the processor's 13-bit instructions.
The program counter is used as the memory address:

PC → Instruction Memory → Instruction

The memory is implemented as a 256-entry array, allowing instructions to be addressed using an 8-bit program counter.

### Instruction Decoder

The instruction decoder extracts the individual fields from the 13-bit instruction.
The decoded fields are:

- Opcode
- Destination register
- Source register 1
- Source register 2
- Direction

These signals are distributed to the control unit, register file, and ALU.

### Control Unit

The control unit receives the three-bit opcode from the instruction decoder and generates the corresponding control signals.

The main control outputs are:
- ALU operation
- Register write enable
- Program counter enable

The control unit therefore determines how the datapath responds to the current instruction.

### Register File

The processor contains eight 8-bit general-purpose registers.
The register file provides:
- Two combinational read ports
- One synchronous write port

The two source register addresses select the operands supplied to the ALU.
The destination register address determines where the ALU result is written when the register-write signal is enabled.

Register organization:
R0, R1, R2, R3, R4, R5, R6, R7

### Arithmetic Logic Unit

The ALU performs the operation selected by the control unit.
Supported operations include:
- Addition
- Subtraction
- AND
- OR
- XOR
- MOV
- Left shift
- Right shift

The ALU receives two 8-bit operands along with the ALU operation and shift direction, and produces an 8-bit result.

## CPU Integration

The individual RTL modules are connected through the top-level CPU module.
The main data-path connections are:

PC → Instruction Memory

Instruction Memory → Instruction Decoder

Instruction Decoder → Control Unit

Instruction Decoder → Register File

Register File → ALU

Control Unit → ALU

ALU → Register File

The result generated by the ALU is fed back to the register file and written into the destination register when the register-write control signal is asserted.



