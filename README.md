# Computer System Architecture – CPU Sim Lab Manual

## Student Details

**Name:** Payal Pandit  
**Roll No.:** 26570045  
**Course / Semester:** B.Sc. Computer Science / 1st Sem  
**College:** Ramanujan College, University of Delhi  
**Academic Session:** 2026-27

---
## Index of Practicals

| S. No. | Practical |
|---:|---|
| 1 | Create a machine based on the Basic Computer architecture |
| 2 | Create the Fetch routine of the instruction cycle |
| 3 | ADD operation on two user-entered numbers |
| 4 | SUBTRACT operation on two user-entered numbers |
| 5 | Logical operations AND, OR, NOT, XOR, NOR, NAND |
| 6 | Memory-reference instructions ADD, LDA, STA, BUN, ISZ |
| 7 | Register-reference instructions CLA, CMA, CME, HLT |
| 8 | Register-reference instructions INC, SPA, SNA, SZE |
| 9 | Register-reference instructions CIR, CIL |
| 10 | Sum of integers until a negative number is read |
| 11 | Sum of integers until zero is read |


----
# Practical 1: Create a Machine (Basic Computer Architecture)

## Aim

To create, in CPU Sim, a machine based on the Basic Computer architecture: its registers, memory, microinstructions, instruction fields and machine instructions.

## Tool Required

CPU Sim 4.0.11 with Java 8 and JavaFX.

## Machine

Mano's Basic Computer

## Theory

A CPU Sim machine is described at the register-transfer level using four main objects:

1. **Hardware Modules** – Registers, condition bits and RAM.
2. **Microinstructions** – Elementary register-transfer operations such as `PC->AR`, `M[AR]->DR` and `AC+DR->AC`.
3. **Fetch Sequence** – The microinstructions executed at the beginning of every instruction cycle.
4. **Machine Instructions** – Instructions containing an opcode, format and execute sequence.

CPU Sim simulates a **microprogrammed control unit**, where each machine instruction is implemented using a sequence of microinstructions.

---

## Procedure

### Step 1 – Start a New Machine

1. Launch CPU Sim using `Cpusim4.bat`.
2. Select **File → New Machine** or press `Ctrl + Shift + N`.
3. Select **Execute → Options** and make sure that bits are indexed from the left.

### Screenshot

> **[Insert Screenshot Here – New Machine window]**

<br><br><br>

---

### Step 2 – Create the Registers

Open:

**Modify → Hardware Modules** (`Ctrl + K`)

Create the following registers with initial value `0`:

| Register | Width | Initial Value |
|---|---:|---:|
| AC | 16 | 0 |
| DR | 16 | 0 |
| AR | 12 | 0 |
| PC | 12 | 0 |
| IR | 16 | 0 |
| E | 1 | 0 |
| TMP | 1 | 0 |
| S | 1 | 0 |

### Screenshot

> **[Insert Screenshot Here – Eight registers in Hardware Modules]**

<br><br><br>

---

### Step 3 – Create Condition Bits and RAM

Create two condition bits:

| Condition Bit | Register | Bit | Halt |
|---|---|---:|---|
| carry-E | E | 0 | No |
| halt-S | S | 0 | Yes |

Then create RAM:

| Name | Length | Cell Size |
|---|---:|---:|
| M | 4096 | 16 |

Click **OK** to save the hardware.

### Screenshot

> **[Insert Screenshot Here – Condition bits]**

<br><br><br>

> **[Insert Screenshot Here – RAM M]**

<br><br><br>

---

## Step 4 – Create Microinstructions

Open:

**Modify → Microinstructions** (`Ctrl + Shift + M`)

### TransferRtoR

| Name | Source | Src Start Bit | Destination | Dest Start Bit | Bits |
|---|---|---:|---|---:|---:|
| PC->AR | PC | 0 | AR | 0 | 12 |
| IR(0-11)->AR | IR | 4 | AR | 0 | 12 |
| AR->PC | AR | 0 | PC | 0 | 12 |
| DR->AC | DR | 0 | AC | 0 | 16 |
| AC(0)->TMP | AC | 15 | TMP | 0 | 1 |
| AC(15)->TMP | AC | 0 | TMP | 0 | 1 |
| E->AC(15) | E | 0 | AC | 0 | 1 |
| E->AC(0) | E | 0 | AC | 15 | 1 |
| TMP->E | TMP | 0 | E | 0 | 1 |

### MemoryAccess

| Name | Direction | Memory | Data | Address |
|---|---|---|---|---|
| M[AR]->IR | read | M | IR | AR |
| M[AR]->DR | read | M | DR | AR |
| AC->M[AR] | write | M | AC | AR |
| DR->M[AR] | write | M | DR | AR |

### Increment

| Name | Register | Delta |
|---|---|---:|
| PC+1->PC | PC | 1 |
| DR+1->DR | DR | 1 |
| AC+1->AC | AC | 1 |

### Arithmetic

| Name | Type | Source 1 | Source 2 | Destination | Carry Bit |
|---|---|---|---|---|---|
| AC+DR->AC,E | ADD | AC | DR | AC | carry-E |

### Logical

| Name | Type | Source 1 | Source 2 | Destination |
|---|---|---|---|---|
| AC^DR->AC | AND | AC | DR | AC |
| AC'->AC | NOT | AC | AC | AC |
| E'->E | NOT | E | E | E |

### Shift

| Name | Source | Destination | Type | Direction | Distance |
|---|---|---|---|---|---:|
| shr | AC | AC | logical | right | 1 |
| shl | AC | AC | logical | left | 1 |

### Set

| Name | Register | Start | Bits | Value |
|---|---|---:|---:|---:|
| 0->AC | AC | 0 | 16 | 0 |
| 0->E | E | 0 | 1 | 0 |

### Test

Create:

- `if(DR!=0)skip-1`
- `if(AC(15)!=0)skip-1`
- `if(AC(15)==0)skip-1`
- `if(AC!=0)skip-1`
- `if(E!=0)skip-1`

### Decode

Create:

`decode-IR`

### SetCondBit

Create:

`1->S(halt)`

### I/O

Create:

- `input-int->AC`
- `output-AC->int`

Click **OK** after entering all microinstructions.

### Screenshot

> **[Insert Screenshot Here – Microinstructions window]**

<br><br><br>

---

## Step 5 – Create Instruction Fields

Open:

**Modify → Machine Instructions** (`Ctrl + M`) → **Edit Fields**

Create these three fields:

| Field | Bits | Used For |
|---|---:|---|
| op | 4 | Opcode of memory-reference instructions |
| addr | 12 | Address part |
| opcode | 16 | Complete code of register-reference and I/O instructions |

Click **OK**.

### Screenshot

> **[Insert Screenshot Here – Edit Fields window]**

<br><br><br>

---

## Step 6 – Create Machine Instructions

Create the following 20 machine instructions:

| Instruction | Opcode |
|---|---|
| AND | `0x0` |
| ADD | `0x1` |
| LDA | `0x2` |
| STA | `0x3` |
| BUN | `0x4` |
| ISZ | `0x6` |
| CLA | `0x7800` |
| CLE | `0x7400` |
| CMA | `0x7200` |
| CME | `0x7100` |
| CIR | `0x7080` |
| CIL | `0x7040` |
| INC | `0x7020` |
| SPA | `0x7010` |
| SNA | `0x7008` |
| SZA | `0x7004` |
| SZE | `0x7002` |
| HLT | `0x7001` |
| INP | `0xF800` |
| OUT | `0xF400` |

For memory-reference instructions, use the format:

`op + addr`

For the remaining instructions, use:

`opcode`

Add the required microinstructions to each **Execute sequence** and finish each sequence with **End**.

### Screenshot

> **[Insert Screenshot Here – Machine Instructions window]**

<br><br><br>

---

## Step 7 – Fetch Sequence and Saving

1. Create the fetch sequence as described in Practical 2.
2. Open **Execute → Options**.
3. Select **PC** as the program counter.
4. Save the machine as:

`BasicComputer.cpu`

### Screenshot

> **[Insert Screenshot Here – Fetch Sequence]**

<br><br><br>

> **[Insert Screenshot Here – Saved BasicComputer.cpu]**

<br><br><br>

---

## Observations

- The machine contains **8 registers**.
- It contains **2 condition bits**.
- RAM contains **4096 cells**, each of **16 bits**.
- The machine contains **33 microinstructions**.
- The machine contains **20 machine instructions**.

## Result

A machine based on the Basic Computer architecture was successfully created in CPU Sim and saved as `BasicComputer.cpu`.
