# Deconstructed Compiler ⚙️

This project is a personal exploration and a practical application of the concepts learned in a Compiler Design course. I thought of deconstructing the entire compilation process to better understand its inner workings. This compiler takes a simple, custom high-level language and compiles it down to **x86 assembly code**.

The project implements all **six logical phases** of a compiler, providing a complete pipeline from source code to executable.

-----

## The Six Phases of Compilation

The compiler is structured around the classic six-phase model, ensuring a modular and logical design.

1.  **Lexical Analysis (Scanning):** The first phase reads the raw source code and converts the stream of characters into a sequence of **tokens**. Each token represents a fundamental building block of the language, like keywords (`if`, `func`), identifiers (`myVar`), operators (`+`, `=`), and literals (`123`, `"hello"`).

2.  **Syntax Analysis (Parsing):** The parser takes the list of tokens from the lexer and verifies that the sequence conforms to the grammatical rules of the language. It constructs an **Abstract Syntax Tree (AST)**, a hierarchical representation of the source code's structure.

3.  **Semantic Analysis:** This phase traverses the AST to check for semantic correctness—rules that are difficult to check with grammar alone. This includes type checking, verifying variable declarations, and matching function arguments.

4.  **Intermediate Code Generation:** After the AST is validated, it's translated into a machine-independent intermediate representation (IR). This project uses **Three-Address Code (TAC)**, where each instruction has at most three operands (e.g., `result = op1 + op2`).

5.  **Code Optimization:** This phase analyzes the IR to produce more efficient code. While not the primary focus, simple optimizations like constant folding could be implemented here to improve performance.

6.  **Code Generation:** The final phase translates the intermediate code into the target machine's assembly language. This project's code generator, `X86CodeGenerator`, converts the TAC into **32-bit x86 assembly** compatible with the NASM assembler.

-----

## Focus: The x86 Code Generator

The `X86CodeGenerator` class is the heart of the final compilation phase. It takes the list of Three-Address Code instructions and systematically converts them into x86 assembly for Linux.

### How It Works

The generator processes each TAC instruction and maps it to a sequence of x86 instructions.

  * **Function Prologue/Epilogue:** For `func_begin` and `func_end`, it generates the standard stack frame setup (`push ebp; mov ebp, esp`) and teardown code.
  * **Arithmetic Operations:** For an operation like `t1 = a + b`, it uses registers (`eax`, `ebx`) to load the operands, performs the operation (`add eax, ebx`), and stores the result back into memory (`mov dword [t1], eax`).
  * **Assignments:** Handles both immediate values (`mov dword [x], 10`) and variable-to-variable assignments (`mov eax, dword [y]; mov dword [x], eax`).
  * **Control Flow:** Translates `if_false`, `goto`, and `label` TAC instructions into conditional jumps (`je`), unconditional jumps (`jmp`), and assembly labels.
  * **System Calls:** Sets up the necessary registers to handle program exit and printing to the console via Linux system calls or by calling C library functions like `printf`.

The output is a complete `.asm` file with `.data` and `.text` sections, ready for assembly and linking.

-----

## How to Run

To compile and run a program using this compiler, you'll need **Python**, **NASM**, and a **32-bit linker** (like `ld`).

1.  **Generate Assembly Code**
    Run the main compiler script with your source file.

    ```bash
    python compiler.py your_program.lang
    ```

    This will produce an assembly file, e.g., `output.asm`.

2.  **Assemble the Code (using NASM)**
    Use NASM to convert the assembly file into a 32-bit ELF object file.

    ```bash
    nasm -f elf32 output.asm -o output.o
    ```

3.  **Link the Object File (using ld)**
    Link the object file to create the final executable.

    ```bash
    ld -m elf_i386 output.o -o my_executable
    ```

    *(Note: If using `printf`, you may need to link against the C standard library.)*

4.  **Run the Executable**

    ```bash
    ./my_executable
    ```
