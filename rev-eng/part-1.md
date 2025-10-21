forward engineering - what you do every day - building a program!

design -> code
- who the hell wrote this code? you!

what gets lost in compilation?
- comments, variable/function names (mangling), structure, optimization levels!

`strip` - use to strip any unnecessary symbols from our compiled binary (no variable names in the executable!)

`objdump -M intel -d <binary>` - display information from object files

### Functions and Frames
a program consists of,
- modules... (most of them are documented so you don't have to reverse engineer them, you usually have to reverse engineer the `main` module)
- that are made of functions... (functions are atomic constructs for reverse engineering, well-defined goal; visualize each functions in "graph" structure, with control flow being the edges)
- that contain blocks...
- of instructions...
- that operate on variables and data structures!


## Data Access
### ELF sections
- **.data:** used for pre-initialized global writable data (such as global arrays with initial values)
- **.rodata:** used for global read-only data (such as string constants)
- **.bss:** used for uninitialized global writable data (global arrays without initial values)

#### Accessing Data
Accessed via rip-relative instructions (if you see an rip-based access, that means we are accessing from the ELF-section)
**Load**: `mov rax, [rip+0x20040]`

### Stacks
Stacks are used to store local variables and call contexts

**rsp**: points to the leftmost/top of the stack
**rbp**: points to the rightmost/bottom of the stack (this is different in different contexts, so cleanup is easier - simply unwind stack upto rbp which is set at the start of a function call)

allocate the stack:
```
push rbp
mov rbp, rsp 
```

deallocate the stack:
```
mov rsp, rbp ; move rsp to the right
pop rbp
```
Note that data is not destroyed by default! 

#### Accessing Data
**push:** store data to top of stack
**pop:** retrieve data from the top of stack
**rsp-relative:** positive offsets `mov [rsp+0x10], rdx`
**rbp-relative:** negative offsets `mov [rbp-0x10], rdx`
**via reference:** `mov rdx, rsp; mov [rdx], 0x41;`

### Heap
dynamically allocated data!


## Static Tools
kaitai struct: file format parser and explorer
nm: lists symbols used/provided by ELF files
strings: dumps ASCII strings found in a file
objdump: simple disassembler
checksec: analyzes security features used by an executable

interactive debuggers: IDA, binary ninja cloud (we'll use this), ghidra

## Dynamic Tools
`ltrace` - traces library calls
`strace` - traces system calls

gdb scripting - I'm very poor with this, better read from a different resource in full detail about gdb

terms to remember
- gdb scripting
- pie: position independent executables
- timeless debugging (gdb, kira)
	- gdb record/replay