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
# How to run

## Prerequisites & Setup
* **Install Java 8 with JavaFX** (for example, **Azul Zulu JDK FX 8**).
* **Download CPU Sim 4.0.11**.

---

## Step-by-Step Execution Guide

1. **Start CPU Sim** by running `Cpusim4.bat`.
2. Go to **File → Open machine…** and choose `Practical_01_Create_Machine/BasicComputer.cpu`.
3. Go to **File → Open text…** and choose the `.a` program file from the practical's folder.
4. Press **Ctrl + 2** to assemble and load the program.
5. Choose how you want to run it:
   * Press **Ctrl + R** to **run** the program.
   * Press **Ctrl + D** to enter **debug mode** (use *Step by Instr* or *Step by Micro*).
6. For programs that require input, type your number into the **yellow console** and press **Enter**.

---

## Observations

- The machine contains **8 registers**.
- It contains **2 condition bits**.
- RAM contains **4096 cells**, each of **16 bits**.
- The machine contains **33 microinstructions**.
- The machine contains **20 machine instructions**.

## Result

A machine based on the Basic Computer architecture was successfully created in CPU Sim and saved as `Basic-Computer.cpu`.
