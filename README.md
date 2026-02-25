# 🖥️ Computer Organization - Complete Exam Prep Notes

> **Interactive visualization + Complete notes for Computer Organization exam**  
> Deploy on Vercel for beautiful visual learning experience!

## 📚 Complete Syllabus Coverage

### 🎯 Unit 1: Basic Organization & Number Systems

#### 1. **Computer Organization - Functional Units**
- **Input Unit**: Keyboard, Mouse, Scanner - converts data to binary
- **Output Unit**: Monitor, Printer - displays results  
- **CPU (Central Processing Unit)**:
  - **ALU (Arithmetic Logic Unit)**: Performs calculations (+, -, ×, ÷) and logic (AND, OR, XOR, NOT, shifts)
  - **Control Unit (CU)**: Fetches instructions, decodes them, generates control signals, manages timing
  - **Registers**: Fast storage inside CPU (AC, PC, IR, MAR, MDR, General Purpose R0-R7)
- **Memory Unit**: RAM, ROM, Cache - stores programs and data
- **Buses**: 
  - **Data Bus**: Transfers data (bidirectional)
  - **Address Bus**: Carries memory addresses (unidirectional, CPU → Memory)
  - **Control Bus**: Carries control signals (Read/Write, Clock, Interrupts)

**Key Concept**: Von Neumann architecture - Program and data both stored in same memory.

---

#### 2. **Number Representation**

**Binary to Decimal**: Multiply each bit by power of 2  
Example: `1011₂ = 1×2³ + 0×2² + 1×2¹ + 1×2⁰ = 8+0+2+1 = 11₁₀`

**Decimal to Binary**: Divide by 2, note remainders  
Example: `13 ÷ 2 = 6 R 1` → `6 ÷ 2 = 3 R 0` → `3 ÷ 2 = 1 R 1` → `1 ÷ 2 = 0 R 1`  
Read remainders **bottom-up**: `1101₂`

**Octal** (base 8): Group binary in **3s** from right  
`101110₂ = (101)(110) = 56₈`

**Hexadecimal** (base 16): Group binary in **4s** from right  
`10111010₂ = (1011)(1010) = BA₁₆`  
(A=10, B=11, C=12, D=13, E=14, F=15)

---

#### 3. **1's and 2's Complement (Signed Numbers)**

**Sign-Magnitude**: MSB = sign (0=positive, 1=negative). Example: `+5 = 0101`, `-5 = 1101`  
❌ **Problem**: Two zeros (+0 and -0), complex addition circuit

**1's Complement**: Flip all bits  
- `+5 = 0101` → `-5 = 1010` (invert all)
- ❌ Still has two zeros: `0000` and `1111`

**2's Complement** ✅ **(Most Common!)**:  
- Formula: **2's complement = 1's complement + 1**  
- `+5 = 0101` → 1's comp = `1010` → 2's comp = `1010 + 1 = 1011` = `-5`
- ✅ **Only ONE zero**, easier circuits, used everywhere!

**Range** (for n bits):
- Unsigned: `0` to `2ⁿ - 1`
- Signed (2's complement): `-2ⁿ⁻¹` to `2ⁿ⁻¹ - 1`
- For 8 bits: **-128 to +127**

**Addition & Subtraction**:
```
A - B = A + (-B) = A + 2's_complement(B)
```
Ignore final carry in 2's complement!

**Overflow Detection**:
- Overflow occurs when **carry into MSB ≠ carry out of MSB**
- OR when **sign of result is wrong** (adding two positives gives negative, etc.)

---

#### 4. **IEEE-754 Floating Point**

**Format**: `Sign (1 bit) | Exponent (8 bits) | Mantissa (23 bits)` = **32 bits total**

**Formula**:  
```
Value = (-1)^Sign × 1.Mantissa × 2^(Exponent - 127)
```

**Steps to convert** -12.75 **to IEEE-754**:
1. Convert to binary: `12 = 1100`, `.75 = .11` → `1100.11`
2. Normalize: `1.10011 × 2³` (move point left to one bit before)
3. **Sign bit**: Negative → `1`
4. **Exponent**: `3 + 127 = 130 = 10000010₂`
5. **Mantissa**: `10011000000000000000000` (23 bits, drop leading 1)
6. **Result**: `1 10000010 10011000000000000000000`

**Special Values**:
- **Zero**: Exp=0, Mantissa=0
- **Infinity**: Exp=255, Mantissa=0
- **NaN** (Not a Number): Exp=255, Mantissa≠0

**Double precision** (64-bit): 1 sign + 11 exp + 52 mantissa

---

### ⚡ Unit 2: Combinational Circuits

#### 5. **Half Adder & Full Adder**

**Half Adder**: Adds 2 bits (A, B)
```
Sum = A ⊕ B (XOR)
Carry = A · B (AND)
```
❌ Cannot handle carry-in from previous stage

**Full Adder**: Adds 3 bits (A, B, Carry_in)
```
Sum = A ⊕ B ⊕ Cin
Cout = (A·B) + (B·Cin) + (A·Cin)  // Majority function
```
✅ Used to build multi-bit adders

---

#### 6. **Binary Adder/Subtractor (4-bit)**

**Ripple Carry Adder**: Chain 4 full adders  
- Carry ripples from LSB to MSB
- ⏱️ **Slow**: Each stage waits for previous carry

**Subtractor**: Use 2's complement
```
A - B = A + 2's_comp(B) = A + (NOT B) + 1
```
**Circuit**: 
- XOR gates controlled by Mode bit (M)
- M=0 → Addition, M=1 → Subtraction
- Set Cin = M (adds the "+1" for 2's complement)

---

#### 7. **Carry Look-Ahead Adder (CLA)**

⚡ **Much faster** than ripple carry!

**Concept**: Calculate all carries simultaneously using:
- **Generate (G)**: `Gi = Ai · Bi` (definitely produces carry)
- **Propagate (P)**: `Pi = Ai ⊕ Bi` (passes carry through)

**Carry equations**:
```
C1 = G0 + P0·C0
C2 = G1 + P1·G0 + P1·P0·C0
C3 = G2 + P2·G1 + P2·P1·G0 + P2·P1·P0·C0
```

All carries computed in **parallel** → Very fast!  
Used in modern CPUs.

---

#### 8. **Multiplexer (MUX) & Demultiplexer (DEMUX)**

**MUX (Data Selector)**: 
- n selection lines choose 1 of 2ⁿ inputs
- **4:1 MUX**: 2 select lines (S1, S0) pick among 4 inputs (I0-I3)
- Formula: `Y = S1'·S0'·I0 + S1'·S0·I1 + S1·S0'·I2 + S1·S0·I3`

**DEMUX (Data Distributor)**:
- Routes 1 input to one of 2ⁿ outputs
- **1:4 DEMUX**: 2 select lines route input to 1 of 4 outputs

**Applications**: Data routing, parallel-to-serial conversion, implementing boolean functions

---

### 🏗️ Unit 3: CPU & Processor Organization

#### 9. **Registers, Bus & Memory Transfer**

**Registers**: Fast storage inside CPU
- **General Purpose**: R0-R7 for calculations
- **Special Purpose**:
  - **PC (Program Counter)**: Address of next instruction
  - **IR (Instruction Register)**: Holds current instruction
  - **MAR (Memory Address Register)**: Memory address to access
  - **MDR/MBR**: Data being read/written
  - **AC (Accumulator)**: Primary ALU operand

**Bus Transfer Operations**:
```
R1 ← R2        ; Copy R2 to R1
R3 ← R1 + R2   ; Add and store
M[AR] ← R1     ; Store R1 to memory at address AR
R2 ← M[AR]     ; Load from memory to R2
```

**Micro-operations happen in ONE clock cycle!**

---

#### 10. **Booth's Algorithm (Signed Multiplication)**

Used for efficiently multiplying **signed** numbers in 2's complement.

**Bit Pairs** → **Action**:
- `01` → Add multiplicand
- `10` → Subtract multiplicand
- `00` or `11` → Just shift (no add/subtract)

**Steps**:
1. Append a 0 to right of multiplier
2. Compare pairs of bits from right
3. Add/Subtract/Shift based on bit pair
4. Arithmetic right shift (preserve sign!)
5. Repeat n times (for n-bit numbers)

**Advantage**: Fewer additions when multiplier has long sequences of 1s  
Example: `0111` (7) requires only 2 ops instead of 3!

---

#### 11. **Processor Organization**

**Register Organization**:
- **General Register Organization**: Multiple general-purpose registers (R0-R15)
  - Modern CPUs: 16-32 registers
  - Operations between any registers
  - Example: ARM, MIPS, x86-64

**Stack Organization**: (see next topic)

---

#### 12. **Stack Organization**

**Stack**: LIFO (Last In, First Out) data structure

**Stack Pointer (SP)**: Points to **top** of stack

**Operations**:
- **PUSH X**: `SP ← SP - 1; M[SP] ← X` (write then decrement)
- **POP X**: `X ← M[SP]; SP ← SP + 1` (read then increment)

**Uses**:
- Function calls (save return address)
- Local variables storage
- Expression evaluation
- Interrupt handling
- Recursion

**Example**:
```
PUSH A    ; Stack: [A]
PUSH B    ; Stack: [A, B]
POP C     ; C=B, Stack: [A]
```

---

### 📋 Unit 4: Instructions & Addressing

#### 13. **Instruction Formats (0, 1, 2, 3 Address)**

**3-Address**: `ADD R1, R2, R3` (R1 = R2 + R3)
- ✅ Fewest instructions, clearest
- ❌ Long instruction (needs many bits)
- **Used in**: RISC (ARM, MIPS, RISC-V)

**2-Address**: `ADD R1, R2` (R1 = R1 + R2)
- ✅ Balanced, most common
- ❌ Destination overwritten
- **Used in**: CISC (x86, x64, VAX)

**1-Address** (Accumulator): `ADD B` (AC = AC + B)
- ✅ Short instructions
- ❌ Many instructions needed, AC bottleneck
- **Used in**: Old CPUs (8085, 8080)

**0-Address** (Stack-based): `ADD` (pops 2, pushes result)
- ✅ Very short opcodes
- ❌ Most instructions, stack overhead
- **Used in**: JVM, Python bytecode, PostScript

**Comparison**: For `X = (A+B) × (C+D)`:
- 3-address: **3 instructions**
- 2-address: **6 instructions**
- 1-address: **7 instructions**
- 0-address: **8 instructions**

---

#### 14. **Addressing Modes**

How to locate operands (Effective Address = EA):

1. **Immediate** `#n`: Operand = constant in instruction  
   - `MOV R1, #25` → R1 = 25 directly
   - ⚡ Fastest (no memory access)
   - Use: Constants, counters

2. **Direct** `[addr]`: EA = address field  
   - `MOV R1, [200]` → R1 = M[200]
   - Use: Global variables, static data

3. **Indirect** `@[addr]`: EA = M[address field]  
   - `MOV R1, @[200]` → R1 = M[M[200]]
   - Use: Pointers, linked lists
   - ⏱️ Slowest (2 memory accesses)

4. **Register** `Rn`: Operand in register  
   - `MOV R1, R2` → R1 = R2
   - ⚡ Fastest (no memory)

5. **Register Indirect** `(Rn)`: EA = Register content  
   - `MOV R1, (R2)` → R1 = M[R2]
   - Use: Pointer dereferencing in C

6. **Indexed** `offset(Rn)`: EA = Rn + offset  
   - `MOV R1, 4(R2)` → R1 = M[R2 + 4]
   - Use: **Arrays!** `arr[i]`, struct members

**More modes**: Base+Index, Relative, Auto-increment/decrement

---

#### 15. **Micro-operations**

Elementary operations executed in **ONE clock cycle**.

**Arithmetic Micro-ops**:
```
R1 ← R2 + R3       ; Add
R1 ← R2 - R3       ; Subtract  
R1 ← R1 + 1        ; Increment
R1 ← R1 - 1        ; Decrement
R1 ← 0             ; Clear
```

**Logical Micro-ops**:
```
R1 ← R2 ∧ R3       ; AND (masking)
R1 ← R2 ∨ R3       ; OR (set bits)
R1 ← R2 ⊕ R3       ; XOR (toggle)
R1 ← NOT R2        ; Complement
```

**Shift Micro-ops**:
- **Logical Shift Left (shl)**: Multiply by 2, fill with 0
- **Logical Shift Right (shr)**: Divide by 2, fill with 0
- **Arithmetic Shift Right (ashr)**: Divide by 2, **preserve sign**
- **Rotate Left/Right (rol/ror)**: Circular shift, no bit loss

**Applications**:
- AND with `0x0F` → Get lower nibble (masking)
- OR with `0x80` → Set MSB (set bits)
- XOR with same value → Clear (A⊕A = 0)
- Left shift by n → Multiply by 2ⁿ
- Right shift by n → Divide by 2ⁿ

---

## 🚀 Deployment Instructions

### Deploy to Vercel:

```bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Deploy
vercel

# Production deployment
vercel --prod
```

Or simply:
1. Push to GitHub
2. Import project on [vercel.com](https://vercel.com)
3. Deploy automatically!

---

## 💡 Quick Exam Tips

### ⭐ Most Important Topics:
1. **2's Complement** - Addition, subtraction, overflow detection
2. **IEEE-754** - Floating point conversion (ask for formula!)
3. **Booth's Algorithm** - Step-by-step multiplication
4. **Instruction Formats** - Comparing 0/1/2/3 address with example
5. **Addressing Modes** - Calculate effective address, use cases
6. **Full Adder** - Truth table, equations
7. **CLA** - Generate/Propagate concept
8. **Micro-operations** - Register transfer notation

### 🎯 Common Mistakes to Avoid:
- ❌ Forgetting to add +1 in 2's complement
- ❌ Wrong exponent bias in IEEE-754 (it's **127**, not 128!)
- ❌ Overflow detection: Check carry IN vs carry OUT of MSB
- ❌ In Booth: Compare **current and previous** bit
- ❌ Stack PUSH decrements SP, POP increments SP (depends on convention!)

### 📝 Formula Sheet:
```
2's complement = 1's complement + 1
Overflow = Cin(MSB) ⊕ Cout(MSB)
IEEE-754 = (-1)^S × 1.M × 2^(E-127)
Booth: Check bit pairs (current, previous)
EA modes: Immediate=#, Direct=[], Indirect=@[], Register=R, Indexed=d(R)
```

---

## 📖 Study Strategy

1. **Visual learning**: Use the interactive app for circuits
2. **Practice by hand**: Do 5-10 problems per topic
3. **Understand WHY**: Don't just memorize, know the reason
4. **Compare alternatives**: 1's vs 2's complement, Ripple vs CLA
5. **Draw diagrams**: Block diagrams, timing diagrams
6. **Real examples**: Connect to actual CPUs (x86, ARM, MIPS)

---

## ✅ Pre-Exam Checklist

- [ ] Can convert between number systems (binary/octal/hex)
- [ ] Can do 2's complement addition/subtraction
- [ ] Can detect overflow correctly
- [ ] Can convert decimal to IEEE-754 and back
- [ ] Can draw Full Adder circuit and truth table
- [ ] Understand CLA advantage over ripple carry
- [ ] Can execute Booth's algorithm step by step
- [ ] Memorized addressing mode formulas
- [ ] Can write micro-operations for given operations
- [ ] Know difference between 0/1/2/3 address formats

---

**Good Luck! 🎓**  
*Visualize → Practice → Understand → Ace the exam!*

---

## 📞 Resources
- **App**: Open `index.html` in browser
- **Deploy**: Push to GitHub & deploy on Vercel
- **Practice**: Work through each interactive example in the app

Made with 💙 for CO Exam Prep