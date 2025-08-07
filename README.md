My Compiler Project: A Journey Through Compilation
This project is my attempt at deconstructing everything I've learned about Compiler Design. I wanted to build a compiler from the ground up, implementing each of the core stages to solidify my understanding of how source code is transformed into executable machine code.

This compiler takes a simple, C-like language and translates it into x86 assembly code.

The Six Phases of Compilation
This project is structured to reflect the six classical phases of a compiler.

1. Lexical Analysis (Scanning)
The first phase scans the raw source code and groups sequences of characters into meaningful units called tokens. For example, the code x = 10; would be broken down into tokens like IDENTIFIER(x), EQUALS, INTEGER(10), and SEMICOLON.

2. Syntax Analysis (Parsing)
The parser takes the stream of tokens from the lexer and verifies that it conforms to the grammatical rules of the source language. It constructs a Parse Tree or an Abstract Syntax Tree (AST), which represents the syntactic structure of the code. This ensures that expressions and statements are correctly formed.

3. Semantic Analysis
This phase checks the AST for semantic consistency. It performs tasks like type checking (e.g., ensuring you don't add a string to an integer), verifying that variables are declared before they are used, and matching function arguments. The output is an annotated AST.

4. Intermediate Code Generation
After semantic analysis, the compiler generates a machine-independent intermediate representation of the source code. This project uses Three-Address Code (TAC), which is a sequence of simple instructions like t1 = a + b. This intermediate form makes the subsequent optimization phase easier to implement.

5. Code Optimization
This is an optional but crucial phase where the intermediate code is analyzed and transformed to produce more efficient code. This can include techniques like constant folding, dead code elimination, and strength reduction. The goal is to improve the performance of the final output without changing its meaning.

6. Code Generation
The final phase takes the (optimized) intermediate code and maps it to the target machine's instruction set. This project's X86CodeGenerator class, as shown in the provided snippet, translates the Three-Address Code into x86 assembly language, handling things like register allocation, memory management, and function call conventions.

How to Use
Clone the repository:

git clone [your-repo-url]

Run the compiler:

python compiler.py your_source_file.lang

Assemble and link the output (using NASM and ld):

nasm -f elf32 output.asm -o output.o
ld -m elf_i386 output.o -o executable

Run your program:

./executable

Example
Here is a small example of the source language:

function main() {
  int a = 10;
  int b = 20;
  int c = a + b;
  print(c);
}

This would be compiled into an output.asm file ready for assembly.
