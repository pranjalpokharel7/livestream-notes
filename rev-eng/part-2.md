**note:** go a bit deeper into hardware layout itself (do VHDL)!

```asm
SECTION .text
ADD rax, rbx // instruction
```

quick revision of yesterday
- disassembly of a binary in gdb, set $eip to an unreachable function
- note: BCD -> 0 to 9 represented in binary

### stack
- any program is assigned a contiguous section of memory called stack
- stack pointer (pointing to top of the stack) contains the smallest address such that any address smaller than the stack pointer is considered garbage in that execution context
- bottom of the stack is the largest address in the stack range
- stack always grows from **larger memory to smaller memory** (i.e. from the bottom of the stack to the top of the stack)
- two operations on the stack which are push and pop

when calling a function,
1. place the arguments on the stack
2. reserve space on the stack for storing return value
3. the "called" function can access these values from the stack
4. FP will keep track of the end of frame for the called function
5. when function exits, SP = FP (note: we do not override the data)

### heap
- grows upward ()4

```
clean_up_blank_lines(file *fp)
remove_duplicate_words(file *fp)
```

stream idea: 
- install ARCH! (this would be priority)
- try helix editor (https://helix-editor.com/)

reading from the stack == popping an item from the stack