# 8BIT RISC Processor
### Instruction Set Architecture (ISA)
---

# 1. Overview
A custom 8-bit RISC processor designed for learning RTL design and computer architecture. The processor follows a simple register-based architecture with fixed-width 13-bit instructions.
Current version focuses only on ALU operations and data movement.

---

# 2. Register Organization

Number of Registers : 8
Register Width : 8 bits
Register Encoding:

| Binary | Register |
|--------|----------|
| 000    |      R0  |
| 001    |      R1  |
| 010    |      R2  |
| 011    |      R3  |
| 100    |      R4  |
| 101    |      R5  |
| 110    |      R6  |
| 111    |      R7  |

---

# 3. Instruction Width

Instruction Width : **13 bits**
Instruction Format:

```
12                                   0
+---------+------+-------+-------+---+
| Opcode  |  RD  |  RS1  |  RS2  | D |
+---------+------+-------+-------+---+
   3 bits   3      3       3      1     
```

Field Description:
- Opcode : Operation to be performed
- RD : Destination Register
- RS1 : Source Register 1
- RS2 : Source Register 2
- D : Shift Direction
---

# 4. Shift Direction Bit
The D bit is used only for SHIFT instructions.

| D | Operation         |
|---|-------------------|
|0  |Logical Left Shift |
|1  |Logical Right Shift|

---

# 5. Opcode Table
| Opcode    | Instruction |
|-----------|-------------|
|  000      |   ADD       |
|  001      |   SUB       |
|  010      |   AND       |
|  011      |   OR        |
|  100      |   XOR       |
|  101      |   MOV       |
|  110      |   SHIFT     |
|  111      | reserved    |

---

# 6. Instruction Definitions

### ADD

```
ADD RD, RS1, RS2

RD ← RS1 + RS2
```

---

### SUB

```
SUB RD, RS1, RS2

RD ← RS1 - RS2
```

---

### AND

```
AND RD, RS1, RS2

RD ← RS1 & RS2
```

---

### OR

```
OR RD, RS1, RS2

RD ← RS1 | RS2
```

---

### XOR

```
XOR RD, RS1, RS2

RD ← RS1 ^ RS2
```

---

### MOV

```
MOV RD, RS1

RD ← RS1
```

**Note:** MOV copies the source register into the destination register.
The source register is **not modified**.

---

### SHIFT

```
SHIFT RD, RS1
```

If D = 0

```
RD ← RS1 << 1
```

If D = 1

```
RD ← RS1 >> 1
```

---

### SPECIAL

Opcode `111` is reserved for future processor extensions.

Possible future instructions include:

- NOP
- HALT
- INC
- DEC
- NOT

---

# Version History

Version 1.0

- 8-bit architecture finalized
- 8 general purpose registers
- 13-bit instruction format defined
- ALU instruction set finalized
- Shift direction bit introduced
