## lesson 1
- asm code directly "corresponds" to CPU instructions
- SIMD (Single Instuction Multiple Data)
	- wash the cars -> 8 workers washing cars at the same time
	- functions in asm to process multiple elements of data in one go
- conventions
	- we will not be using *intrinsics*
	- no inline assembly - lack of general compiler support
	- all *mnemonics* are lower case (upper case is reserved for macros), eg: `mov rax, 3`  and not `MOV rax, 3` -> `mov` is a mnemonic and is lower case
- we will generally not be working with general purpose registers for ffmpeg since they are not directly used for SIMD instruction

### vector registers

- mm registers: MMX, 64-bit sized, historical reasons not used much more
- **xmm registers:** 128-bit sized, widely available
- ymm registers: 256-bit sized
- zmm registers: 512-bit sized

`x86inc.asm` - helps in 

**revision:** the .text section contains our code, and the .data section contains global variables/constants

### compilation steps

1. download nasm
2. [download](https://raw.githubusercontent.com/FFmpeg/FFmpeg/master/libavutil/x86/x86inc.asm) the `x86inc.asm` and place it in the same directory as your assembly file (`nasm` will detect it)
3. compile assembly to object file (which you can link later), we need to define some preprocessor symbols carefully
   ```bash
   nasm -f elf64 -I. \
            -DARCH_X86_64=1 \
            -DHAVE_ALIGNED_STACK=1 \
            -DPIC=1 \
            -Dprivate_prefix=our \
            simd.asm -o add_values.o

	```

4. Link the object files together - sample source file
   ```c
   #include <stdint.h>
#include <stdio.h>

extern void our_add_values_sse2(uint8_t *src, const uint8_t *src2);

int main() {
  uint8_t a[16] = {1, 2, 3, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
  uint8_t b[16] = {5, 6, 7, 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};

  our_add_values_sse2(a, b);

  for (int i = 0; i < 4; i++)
    printf("%u\t", a[i]);
}
```

5. Compilation and linking steps
   ```bash
   $ gcc -c main.c -o main.o
   $ gcc main.o add_values.o -o program
```


## lesson 2
### loops and conditionals

flags registers
- zero flag
- sign flag
- overflow flag

**note to self:** 
- run debugger to inspect flags and GPR
- go into details about x86 flag registers (including overflow, signed, zero) - in a different session
- ~~display `.rodata` section in disassembly~~


"not writing assembly to match C loops exactly, we write loops to make them as fast as possible in assembly." - motto of ffmpeg - assembly is a tool to execute things faster!

offsets - bytes between consecutive elements in memory

```c
uint32_t data[3];
int i;
for(i = 0; i < 3; i++) {
    data[i];
}

uint32_t data[4];
int i;
for(i = 0; i < 3; i++) {
	usize scale = sizeof(*data);
    *data + (i * scale) + 32; // this is equivalent to data[i];
}
```

```
| 32-bit space 0 | 32-bit space 1 | 32-bit space 3 | 32-bit space 4 | 
```