# Fundamentals

> Buffer overflows are errors that allow data that is too large to fit into a buffer of the operating system's memory that is not large enough, thereby overflowing this buffer. As a result of this mishandling, the memory of other functions of the executed program is overwritten, potentially creating a security vulnerability.

File formats binary :

* Portable Executable Format (**PE**) - Microsoft Platforms
* Executable and Linking Format (**ELF**) - UNIX

## The Memory

<figure><img src="../../.gitbook/assets/image (137).png" alt=""><figcaption><p>memory</p></figcaption></figure>

* **.text** : contains the actual assembler instructions of the program. This area can be read-only to prevent the process from accidentally modifying its instructions. Any attempt to write to this area will inevitably result in a segmentation fault.
* **.data** : contains global and static variables that are explicitly initialized by the program.
* **.bss** : Several compilers and linkers use the .bss section as part of the data segment, which contains statically allocated variables represented exclusively by 0 bits.
* **Heap** : is allocated from this area. This area starts at the end of the ".bss" segment and grows to the higher memory addresses.
* **Stack** : is a **Last-In-First-Out** data structure in which the return addresses, parameters, and, depending on the compiler options, frame pointers are stored. C/C++ local variables are stored here, and you can even copy code to the stack. The Stack is a defined area in RAM. The linker reserves this area and usually places the stack in RAM's lower area above the global and static variables. The contents are accessed via the stack pointer, set to the upper end of the stack during initialization. During execution, the allocated part of the stack grows down to the lower memory addresses.

**About Stack**&#x20;

Modern memory protections (_DEP/ASLR_) would prevent the damaged caused by buffer overflows. **DEP** (Data Execution Prevention), marked regions of memory "Read-Only".  The read-only memory regions is where some user-input is stored (Example: _The Stack_), so the idea behind DEP was to prevent users from uploading shellcode to memory and then setting the instruction pointer to the shellcode. **Hackers started utilizing ROP** (Return Oriented Programming) to get around this, as it allowed them to upload the shellcode to an executable space and use existing calls to execute it. With ROP, the attacker needs to know the memory addresses where things are stored, so the defense against it was to implement ASLR (Address Space Layout Randomization) which randomizes where everything is stored making ROP more difficult.

## Vulnerable functions in C

* strcpy
* gets
* sprintf
* scanf
* strcat

## CPU Registers

### Data registers

| 32-bit Register | 64-bit Register |                                                 Description                                                 |
| :-------------: | :-------------: | :---------------------------------------------------------------------------------------------------------: |
|     **EAX**     |     **RAX**     |                      Accumulator is used in input/output and for arithmetic operations                      |
|     **EBX**     |     **RBX**     |                                      Base is used in indexed addressing                                     |
|     **ECX**     |     **RCX**     |                            Counter is used to rotate instructions and count loops                           |
|     **EDX**     |     **RDX**     | Data is used for I/O and in arithmetic operations for multiply and divide operations involving large values |

### Pointer registers

| 32-bit Register | 64-bit Register |                                                                                                                                      Description                                                                                                                                     |
| :-------------: | :-------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     **EIP**     |     **RIP**     |                                                               Instruction Pointer for the stack. In other words, it tells the computer where to go next to execute the next command and controls the flow of a program.                                                              |
|     **ESP**     |     **RSP**     |                                                                                                                     Stack Pointer points to the top of the stack                                                                                                                     |
|     **EBP**     |     **RBP**     | Base Pointer is also known as Stack Base Pointer or Frame Pointer thats points to the base of the stack ; it stores the address of the beginning of the stack frame. Thus, the current stack frame is located between the address contained in EBP and the address contained in ESP. |

### Stack frames

Since the stack starts with a high address and grows down to low memory addresses as values are added, the **Base Pointer** points to the **beginning** (base) of the stack in contrast to the **Stack Pointer**, which points to the **top of the stack**.

As the **stack grows**, it is logically **divided** into regions called **Stack Frames**, which allocate the required memory in the stack for the corresponding function. **A stack frame** defines a **frame of data with the beginning** (EBP) **and the end** (ESP) that is pushed onto the stack when a function is called.

<figure><img src="../../.gitbook/assets/image (149).png" alt=""><figcaption><p>stack</p></figcaption></figure>

## Prevention

### Canaries

The **canaries** are known values written to the stack between buffer and control data to detect buffer overflows. The principle is that in case of a buffer overflow, the canary would be overwritten first and that the operating system checks during runtime that the canary is present and unaltered.

### Address Space Layout Randomization (ASLR)

Address Space Layout Randomization (**ASLR**) is a security mechanism against buffer overflows. It makes some types of attacks more difficult by making it difficult to find target addresses in memory. The operating system uses ASLR to hide the relevant memory addresses from us. So the addresses need to be guessed, where a wrong address most likely causes a crash of the program, and accordingly, only one attempt exists.

### Data Execution Prevention (DEP)

**DEP** is a security feature available in Windows XP, and later with Service Pack 2 (SP2) and above, programs are monitored during execution to ensure that they access memory areas cleanly. DEP terminates the program if a program attempts to call or access the program code in an unauthorized manner.

### Containers

An even further defense mechanism is called a **container**, which is another layer of Data Execution Prevention. The container attempts to identify all possible results of code from data within the buffer (or the data segment) and then prevent the application from calling external functions in shared objects from the inside of the buffer. A version of this has been implemented in Cisco Security Agent, or CSA. Linux's GrSec and PaX kernel patches also implement their own version of contained memory space.
