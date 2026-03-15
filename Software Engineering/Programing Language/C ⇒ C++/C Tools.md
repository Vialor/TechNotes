## gcc/g++

> gcc: GUN Compiler Collection (by Richard Matthew Stallman)

Code ⇒(preprocessor) ⇒(compiler) Asm program(.s file) ⇒(assembler) Object Program(.o file) ⇒(linker) Executable Program
```shell
-E                       Preprocess only; do not compile, assemble or link.
-S                       Compile only; do not assemble or link.
-c                       Compile and assemble, but do not link.
-o <file>                Place the output into <file>.
gcc -c main. -o <name of the object file>:
compile and assemble c files, output object files (binary machine code, end with .o)
gcc *.o -o <name of the executable>:
link all the object files to an executable file (or compile all the c files into object files and then link them)

# useful combinations
gcc -Og -S main.c # Og specify the optimization level
```
Use g++ for cpp: `g++ main.cpp --std=c++11`
## Disassembler
> Machine code => assembly code

objdump: `objdump -d filename`
gdb: `disassemble filename`
## GDB: debugger
```
disassemble filename
```
## Use NASM to compile Assembly Language
## Virtual Machine

> Virtual Machine: the virtualization/emulation of a computer system
qemu/bochs