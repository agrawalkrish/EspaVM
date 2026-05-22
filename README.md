# LC-3 Emulator in C++

A simple C++ implementation of an **LC-3 virtual machine/emulator**.  
This project loads LC-3 object image files into memory, emulates the LC-3 instruction cycle, handles memory-mapped keyboard input, and executes supported LC-3 instructions and trap routines.

---

## 📌 Project Overview

The LC-3, or **Little Computer 3**, is an educational 16-bit computer architecture commonly used to understand computer organization, assembly language, instruction execution, memory, registers, and basic operating-system-like trap routines.

This emulator demonstrates:

- 16-bit memory and register simulation
- Instruction fetch-decode-execute cycle
- LC-3 opcode handling
- Memory-mapped keyboard registers
- Trap routines for input/output
- Cross-platform terminal input handling for Windows and POSIX systems
- Loading LC-3 binary image files into virtual memory

---

## ✨ Features

### 🧠 Memory Management

- Simulates `2^16` memory locations
- Supports 16-bit memory addressing
- Provides safe memory read/write methods
- Implements memory-mapped keyboard registers:
  - `MR_KBSR` — Keyboard Status Register
  - `MR_KBDR` — Keyboard Data Register

### ⌨️ Keyboard Input Handling

The emulator disables normal terminal buffering so keyboard input can be detected during execution.

Supported platforms:

- Windows
- Linux/macOS/POSIX-based systems

### 📂 LC-3 Image Loading

The emulator can load LC-3 binary image files into memory.

Each image file contains:

1. An origin address
2. Program instructions/data loaded from that origin

The emulator handles endian conversion using a `swap16` helper.

### ⚙️ CPU Register Simulation

The emulator includes the following LC-3 registers:

| Register | Purpose |
|---|---|
| `R0`–`R7` | General-purpose registers |
| `PC` | Program Counter |
| `COND` | Condition flags |
| `COUNT` | Register count marker |

### 🚩 Condition Flags

The emulator supports LC-3 condition flags:

| Flag | Meaning |
|---|---|
| `FL_POS` | Positive |
| `FL_ZRO` | Zero |
| `FL_NEG` | Negative |

Condition flags are updated after arithmetic, logical, and load operations.

---

## 🧾 Supported LC-3 Instructions

The emulator currently implements the following LC-3 opcodes:

| Opcode | Instruction | Status |
|---|---|---|
| `BR` | Conditional Branch | Implemented |
| `ADD` | Addition | Implemented |
| `LD` | Load | Implemented |
| `ST` | Store | Implemented |
| `JSR` / `JSRR` | Jump to Subroutine | Implemented |
| `AND` | Bitwise AND | Implemented |
| `LDR` | Load Register | Implemented |
| `STR` | Store Register | Implemented |
| `RTI` | Return from Interrupt | Aborts / not fully supported |
| `NOT` | Bitwise NOT | Implemented |
| `LDI` | Load Indirect | Implemented |
| `STI` | Store Indirect | Implemented |
| `JMP` / `RET` | Jump | Implemented |
| `RES` | Reserved | Placeholder |
| `LEA` | Load Effective Address | Implemented |
| `TRAP` | Trap Routine | Implemented |

---

## 🧰 Supported Trap Routines

| Trap Code | Routine | Description |
|---|---|---|
| `0x20` | `GETC` | Reads one character from keyboard |
| `0x21` | `OUT` | Prints one character |
| `0x22` | `PUTS` | Prints a null-terminated string |
| `0x23` | `IN` | Prompts user and reads one character |
| `0x25` | `HALT` | Stops program execution |

---

## 🏗️ Project Structure

```text
.
├── main.cpp      # LC-3 emulator source code
└── README.md     # Project documentation
```

The project is currently implemented in a single C++ source file.

---

## ⚙️ Requirements

You need a C++ compiler that supports modern C++.

Recommended:

- `g++`
- `clang++`
- MSVC on Windows

---

## 🚀 Build Instructions

### Linux/macOS

```bash
g++ main.cpp -o lc3
```

### Windows using MinGW

```bash
g++ main.cpp -o lc3.exe
```

### Windows using MSVC

```bash
cl main.cpp /Fe:lc3.exe
```

---

## ▶️ Usage

Run the emulator with one or more LC-3 image files:

```bash
./lc3 program.obj
```

On Windows:

```bash
lc3.exe program.obj
```

You can also pass multiple image files:

```bash
./lc3 file1.obj file2.obj
```

If no image file is provided, the program displays:

```text
Usage: lc3 [image-file1] ...
```

---

## 🔁 How It Works

The emulator follows the standard LC-3 execution cycle:

1. Load LC-3 image file into memory
2. Set the program counter to `0x3000`
3. Fetch instruction from memory
4. Decode opcode
5. Execute the corresponding instruction
6. Update registers, memory, and condition flags
7. Continue until a `HALT` trap or error occurs

Main execution loop:

```cpp
while (running)
{
    unsigned short instr = Memory::read(reg[R_PC]++);
    unsigned short op = instr >> 12;

    switch (op)
    {
        // instruction handlers
    }
}
```

---

## ⚠️ Important Note

Before compiling, check the source file for accidental stray text.  
For example, there is a standalone `Hell` token inside the `add()` function in the uploaded source. This will cause a compilation error and should be removed.

Problematic section:

```cpp
short unsigned int imm_flag = (instr >> 5) & 0x1;
Hell
if (imm_flag)
```

Correct version:

```cpp
short unsigned int imm_flag = (instr >> 5) & 0x1;

if (imm_flag)
```

---

## 🧪 Example

After compiling:

```bash
g++ main.cpp -o lc3
./lc3 hello.obj
```

Expected behavior depends on the LC-3 object file being executed. If the loaded LC-3 program prints text, uses keyboard input, or halts, the emulator will handle those actions through trap routines.

---

## 🛠️ Possible Improvements

Future improvements could include:

- Add full `RTI` support
- Improve error handling for invalid opcodes
- Add debug mode for registers and memory
- Add instruction tracing
- Add unit tests for each opcode
- Support command-line flags
- Improve `PUTS` handling for full LC-3 compatibility
- Separate the emulator into multiple files/classes
- Add CMake build support

---

## 📚 Concepts Demonstrated

This project is useful for learning:

- Computer architecture
- Virtual machines
- Instruction set emulation
- Binary file loading
- Endianness
- Memory-mapped I/O
- Terminal input buffering
- CPU registers and condition flags
- Fetch-decode-execute cycle

---


## 👤 Author

Created by **Krishna Agrawal** and **Urdhva Borse**.

---
## Future Plan:
Make the *Asteroids* game from Atari somehow run  and gui some way

---
The name **Espa** comes from the K-pop group Aespa (don't mind the name)
