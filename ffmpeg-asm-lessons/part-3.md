## Revision
- looping 
	- unconditional (using `jmp` instruction to jump to a label)
	- conditional (using flag registers)
- constants
	- defined in the `RODATA` section (read-only data)
- offsets
	- we can perform arithmetic operations before memory load
	- "x86asm provides the constant mmsize, which lets you know the size of the SIMD register you are working with" - xmm registers, them mmsize is 128
- loading form address
	- lea: load effective address
	- memory location is accessed through square brackets `[r1q + i]`
- **assignment:** load a constant and add the values to a SIMD vector in a loop
	- this constant would also be a 128-bit memory (16 `uint_8`s)
	- we'll add each to corresponding byte in a SIMD register
	- we'll do this in the next stream where we'll code a bunch of x86 programs (after I fix my ssd issue - hopefully it isn't serious)

## Lesson 3

### Instruction Sets
- every CPU has a list of instructions that it can run - this is the instruction set
- SSE (Streaming SIMD Extensions) - 128-bit registers
- "function pointers are C by default and are replaced with a particular instruction set variant" - use macros to re-assign function pointers (analogy)

- note: revisit the "pointer offset trickery" example tomorrow

### alignment (aside)

64-bit registers
```c
struct unaligned {
	uint_64t first; // 8 bytes
	uint_8t a; // 1 byte + 7 byte of padding
	uint_64t second; // 8 bytes
}
```

![[Pasted image 20250920220148.png]]

### Range Expansion
overflow: 0 to 255 then back to 0
**eg:** 255 + 1 = 0

- punpcklbw (**p**acked **unpack** **l**ow **b**ytes to **w**ords)
	- create a lesson 4 ourselves so that we can understand why we're using this instruction

```c
uint8_t a = 0b11111110;
uint16_t b = 0b11111111111111110;

0XXXXXXX & 10000000
```