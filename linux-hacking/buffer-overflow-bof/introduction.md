# Introduction

## Brief

<details>
<summary>What is a Buffer Overflow ?</summary>

In short, buffer overflows are caused by **incorrect program code**, which cannot process too large amounts of data correctly by the CPU and can, therefore, manipulate the CPU's processing. Suppose too much data is written to a reserved memory buffer or stack that is not limited, for example. In that case, specific registers will be overwritten, which may allow code to be executed.

A buffer overflow can cause the program to **crash**, **corrupt data**, or harm data structures in the program's runtime. The last of these can overwrite the specific program's return address with arbitrary data, allowing an attacker to **execute commands with the privileges of the process vulnerable** to the buffer overflow by passing arbitrary machine code. This code is usually intended to give us more convenient access to the system to use it for our own purposes. Such buffer overflows in common servers, and Internet worms also exploit client software.

The **most significant cause** of buffer overflows is the use of **programming languages that do not automatically monitor limits of memory buffer or stack to prevent** (stack-based) buffer overflow. These include the C and C++ languages, which emphasize performance and do not require monitoring.

</details>

### Central Processing Unit (CPU)

<details>

<summary>CPU Architecture</summary>

The Central Processing Unit (**CPU**) is the **functional unit in a computer** that provides the actual **processing power**. It is responsible for **processing** **information** and **controlling** the **processing** **operations**. To do this, the CPU fetches commands from memory one after the other and initiates data processing. Each CPU has an architecture on which it was built. The best-known **CPU architectures** are:

* x86/i386 - (AMD & Intel)
* x86-64/amd64 - (Microsoft & Sun)
* ARM - (Acorn) Each of these CPU architectures is built in a specific way, called **Instruction Set Architecture** (ISA), which the CPU uses to execute its processes. ISA, therefore, describes the behavior of a CPU concerning the instruction set used. The instruction sets are defined so that they are independent of a specific implementation. Above all, ISA gives us the possibility to understand the unified behavior of **machine code** in assembly language concerning **registers**, **data types**, etc.

There are four different types of ISA:

* **CISC** - Complex Instruction Set Computing
* **RISC** - Reduced Instruction Set Computing
* **VLIW** - Very Long Instruction Word
* **EPIC** - Explicitly Parallel Instruction Computing

In the Von-Neumann architecture, the most important units, the **Arithmetical Logical Unit** (**ALU**) and **Control Unit** (**CU**), are combined in the actual Central Processing Unit (CPU).&#x20;

The CPU is responsible for executing the **instructions** and for **flow control**.&#x20;

The instructions are executed one after the other, step by step.&#x20;

The commands and data are fetched from memory by the **CU**.&#x20;

The connection between processor, memory, and input/output unit is called a **bus system**, which is not mentioned in the original Von-Neumann architecture but plays an essential role in practice.&#x20;

In the Von-Neumann architecture, all instructions and data are transferred via the **bus system**.&#x20;

</details>

<figure><img src="../../.gitbook/assets/von_neumann3 (1).png" alt=""><figcaption><p>CPU Architecture</p></figcaption></figure>

### Memory

<details>

<summary>Memory</summary>

It can be divided into **2 categories** :&#x20;

* **Primary** memory
  * The **Cache** and **Random Access Memory** (RAM).&#x20;
  * We can think of it as _leaving something at one of our friends to pick it up again later_. But for this, it is necessary to _know the friend's address_ to pick up what we have left behind. It is the same as RAM. RAM describes a memory type whose memory allocations can be accessed directly and randomly by their **memory addresses**. The **cache** is integrated into the processor and serves as a buffer, which in the best case, ensures that the processor is always fed with data and program code. Before the program code and data enter the processor for processing, the RAM serves as data storage. The size of the RAM determines the amount of data that can be stored for the processor. However, when the primary memory loses power, all stored contents are lost.
* **Secondary** memory
  * The external data storage, such as **HDD/SSD**, **Flash Drives** and **CD/DVD-ROMs** of a computer, which is not directly accessed by the CPU, but via the **I/O** interfaces. In other words, it is a mass storage device. It is used to permanently store data that does not need to be processed at the moment. Compared to **primary memory**, it has a **higher storage capacity**, can store data permanently even without a power supply, and works much slower.

</details>

### Control Unit

<details>

<summary>Control Unit (CU)</summary>

The Control Unit (CU) is responsible for the **correct interworking of the processor's individual parts**. An internal bus connection is used for the tasks of the CU.&#x20;

The tasks of the CU can be summarised as follows:

* Reading data from the RAM
* Saving data in RAM
* Provide, decode and execute an instruction
* Processing the inputs from peripheral devices
* Processing of outputs to peripheral devices
* Interrupt control
* Monitoring of the entire system

</details>

### Instruction Cycle

|                       Instruction                      |                                                                                                                                                           Description                                                                                                                                                           |
| :----------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                 <ol><li>FETCH</li></ol>                |                                                                              The next machine instruction address is read from the Instruction Address Register (IAR). It is then loaded from the Cache or RAM into the Instruction Register (IR).                                                                              |
|           <ol start="2"><li>DECODE</li></ol>           |                                                                                                         The instruction decoder converts the instructions and starts the necessary circuits to execute the instruction.                                                                                                         |
|       <ol start="3"><li>FETCH OPERANDS</li></ol>       |                                                                                                       If further data have to be loaded for execution, these are loaded from the cache or RAM into the working registers.                                                                                                       |
|           <ol start="4"><li>EXECUTE</li></ol>          | The instruction is executed. This can be, for example, operations in the ALU, a jump in the program, the writing back of results into the working registers, or the control of peripheral devices. Depending on the result of some instructions, the status register is set, which can be evaluated by subsequent instructions. |
| <ol start="5"><li>UPDATE INSTRUCTION POINTER</li></ol> |                                                                           If no jump instruction has been executed in the EXECUTE phase, the IAR is now increased by the length of the instruction so that it points to the next machine instruction.                                                                           |
