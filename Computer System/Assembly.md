# Instruction Sets Architecture ISA
**ISA**: The parts of a processor design that one needs to understand to write assembly/machine code
**Microarchitecture**: The underlying implementation of ISA
**Assembly Code**: The text representation of machine code

**CISC**: complex instruction set computer
	x86-64: from 32-bit to 64-bit CPU and OS
**RISC**: reduced instruction set computer
	ARM
# Assembly Code: Intel Grammar
**Intel Syntax** and **AT&T Syntax** are equivalent CISC languages
Useful tools: [[C Tools]]
## Basics
### Operand Types:
**Immediate**: constant integers
	e.g. $0x400 $-533
**Register**: one of 16 integer registers (e in eax is for 32 bits, r is for 64 bits)
	e.g. %rax %r13
	%rsp reserved for special purpose
**Memory**: 8 consecutive bytes of memory at address given by registers
	e.g. ($rax) 8(%rbp)
	complete **address mode expression**: `0x8(%rdx,%rcx,4) == 0x8+(%rdx)+(%rcx)*4`
### `movq src, dest`

| movq grammar | C example    |
| ------------ | ------------ |
| movq imm reg | temp = 4;    |
| movq imm mem | *p=-147;     |
| movq reg reg | temp2=temp1; |
| movq reg mem | *p=temp;     |
| movq mem reg | temp=*p;     |
### Arithmetic & Logic
 #### `leaq src, dest`
load effective address: set dest to the address denoted by src (src is in address mode expression)
leaq is used a lot for arithmetic

```
addq src, dest            dest = dest + src
subq                      subtract
imulq                     multiply
salq                      left shift
sarq                      right shift

xorq                      dest = dest ^ src
andq                      and
orq                       or

incq                      dest++
decq                      dest--
negq                      dest = -dest
notq                      dest = ~dest
```
## Controls
