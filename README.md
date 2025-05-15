# LC-3 Virtual Machine

This is a simple virtual machine implementation in C, designed to emulate the LC-3 (Little Computer 3) architecture. It provides the core functionality to load and execute LC-3 assembly programs.

## Features

* **LC-3 Instruction Set:** Implements the LC-3 instruction set, including:
    * Arithmetic operations (ADD, AND, NOT)
    * Branching (BR)
    * Jump operations (JMP, JSR)
    * Load and store operations (LD, LDI, LDR, LEA, ST, STI, STR)
    * Trap routines (GETC, OUT, PUTS, IN, PUTSP, HALT)
* **Memory Management:** Simulates the LC-3's 16-bit address space.
* **Registers:** Emulates the LC-3's general-purpose registers, program counter (PC), and condition code register.
* **Input/Output:** Provides basic I/O functionality through trap routines.
* **Program Loading:** Loads LC-3 object files (.obj) into memory.
* **Basic Error Handling:** Handles the case where the program file cannot be loaded.
* **Interrupt Handling:** Sets up a basic interrupt handler for `SIGINT` (Ctrl+C) to restore terminal settings and exit gracefully.
* **Input Buffering:** Disables terminal input buffering for character-by-character input as required by some trap routines.

## Prerequisites

* **C Compiler:** A C compiler (like GCC) is required to compile the code.
* **Standard C Libraries:** The code uses standard C libraries.

## Getting Started

1.  **Save the code:** Save the provided C code into a file named `lc3vm.c`.

2.  **Compile the code:** Use your C compiler to compile the `lc3vm.c` file:

    ```bash
    gcc lc3vm.c -o lc3
    ```

3.  **Run the virtual machine:**

    ```bash
    ./lc3 <program_file>
    ```

    Replace `<program_file>` with the path to the LC-3 object file (.obj) that you want to execute.

## Code Explanation

The C code implements the following:

1.  **Includes:**
    * Standard C libraries (`stdlib.h`, `stdint.h`, `stdio.h`, `string.h`)
    * System-related headers (`sys/time.h`, `unistd.h`, `sys/termios.h`, `signal.h`)

2.  **Memory:**
    * `memory`: A `uint16_t` array representing the LC-3's 65536-word memory.

3.  **Registers:**
    * `reg`: A `uint16_t` array representing the LC-3's registers.
    * Defines an `enum` for register names (R_R0 to R_COUNT).

4.  **Opcode Definitions:**
    * Defines an `enum` for LC-3 opcodes (OP_BR, OP_ADD, etc.).

5.  **Condition Flags:**
    * Defines an `enum` for the condition code flags (FLAG_POS, FLAG_ZRO, FLAG_NEG).

6.  **Trap Code Definitions:**
    * Defines an `enum` for LC-3 trap codes (TRAP_GETC, TRAP_OUT, etc.).
    * Defines memory locations for memory-mapped registers

7.  **Helper Functions:**
    * `update_flags()`: Updates the condition code register based on the result of an operation.
    * `sign_extend()`: Performs sign extension on a value.
    * `swap16()`: Swaps the bytes of a 16-bit value (for handling endianness).
    * `check_key()`: Checks if a key has been pressed.
    * `mem_read()`: Reads a 16-bit value from memory, simulating memory-mapped I/O for the keyboard.
    * `mem_write()`: Writes a 16-bit value to memory.
    * `load_program_from_file()`: Loads an LC-3 object file into the simulated memory.

8.  **Opcode Functions:**
    * Functions for each LC-3 opcode (e.g., `op_add()`, `op_and()`, `op_br()`, etc.) that implement the instruction's logic.

9.  **Trap Handler Functions:**
    * Functions for each LC-3 trap code (e.g., `trap_puts()`, `trap_getc()`, etc.) that implement the trap's functionality.

10. **Input Buffering Functions:**
    * `disable_input_buffering()`: Disables terminal input buffering.
    * `restore_input_buffering()`: Restores terminal input buffering.

11. **Interrupt Handler:**
    * `handle_interrupt()`:  A signal handler for `SIGINT` (Ctrl+C) to restore terminal settings before exiting.

12. **Setup Function**
     * `setup()`: Sets up the signal handler and disables input buffering

13. **`main()` Function:**
    * Parses the command-line argument (the program file).
    * Loads the program into memory.
    * Initializes the registers (PC, etc.).
    * Enters the main execution loop:
        * Fetches the next instruction.
        * Decodes the instruction.
        * Executes the instruction by calling the appropriate opcode function.
        * Handles trap routines.
    * Restores terminal input buffering before exiting.

##  LC-3 Concepts

This implementation assumes you have a basic understanding of the LC-3 architecture, including:

* **Memory organization**
* **Registers**
* **Instruction set architecture**
* **Trap routines**

##  Additional Notes

* **Error Handling:** The error handling in this code is basic.  A more robust implementation would include more detailed error messages and handling of various error conditions.
* **Input/Output:** The I/O is handled through trap routines, which interact with the terminal.
* **Object File Format:** The `load_program_from_file()` function expects a specific LC-3 object file format (.obj).
* **Endianness:** The code uses `swap16()` to handle potential endianness issues when reading the object file.
* **Interrupts:** The code provides a basic interrupt handler for Ctrl+C, but it does not implement the full LC-3 interrupt mechanism.
* **Further Development:** This is a basic LC-3 virtual machine.  It could be extended to support more advanced features, such as a debugger, a graphical interface, or support for LC-3 assembly language.
