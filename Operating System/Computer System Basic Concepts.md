# Computer System Operation
A modern general-purpose computer system consists of **one or more CPUs** and **a number of device controllers** connected through the **system bus** that provides access to **common shared memory**.
**Device controller:** a hardware e.g. disk controller(disks), USB controller(mouse, keyboard, printer), video adaptor(monitor)
Typically, OS contains a **device driver** for each device controller: **device drivers** understand the varying natures of different device controller and expose an uniform interface to the rest of OS.
**Memory controller** ensures CPU & device controllers to have orderly access to shared memory
- **Bootstrap Program:** The initial program that runs when the computers start up or reboot
    It is stored in ROM; it loads and starts OS; it locates and loads the OS kernel into memory.
- **Interrupt:** an action from software or hardware to affect the current task of CPU in a certain way, to be specific:
    A hardware may trigger an interrupt by sending a **signal** to CPU by the **system bus.**
    A software may trigger an interrupt by executing a **system call**.
    When CPU is interrupted, it stops current task and immediately transfers execution to a fixed location to allow **Interrupt Service Routine** to execute. Once completed, the CPU resumes the previously interrupted task.
    
# Storage Structure
Storage is one of the I/O devices.

|                                                |                                                                      |
| ---------------------------------------------- | -------------------------------------------------------------------- |
| Registers                                      |                                                                      |
| Cache (Located directly on the processor chip) |                                                                      |
| Main Memory (Random Access Memory, RAM)        | ↑ Above: Expensive, Smaller Size, Faster Access, Volatile            |
| Electronic Disk                                | ↓ Below: Larger Size, Slower Access, Non Volatile (Secondary Memory) |
| Magnetic Disk                                  |                                                                      |
| Optical Disk                                   |                                                                      |
| Magnetic Tapes                                 |                                                                      |
**Volatile:** lose contents once power off
# Working of an I/O Operation
Note: device controller maintains a **local buffer storage** and a set of **special purpose registers**
1. Device driver loads appropriate registers from the device controller
2. Device controller, in turn, examine the contents of the registers to determine the next action
3. Device controller starts transferring data from the device to its local buffer
4. Once the data transfer complete, device controller issues an interrupt to inform device driver
5. Device driver returns control to the OS
- Direct Memory Access DMA:
    
    What: a capability provided by some computer bus architectures that allows data to be sent directly from an attached device (such as a disk drive) to the memory on the computer's motherboard without intervention of CPU. (Step 1 to 3 remain the same)
    
    Why: The above interrupt-driven I/O is fine for small data movement, but can cause high overhead for bulk data movement. (former one will have one interrupt per byte, yet latter one(DMA) will gave only one interrupt per block)
    
    (also have the benefit to allow CPU to do other tasks)
    
# Computer System Architecture
Types of Computer System based on number of General Purpose Processor
- **Single Processor System:** One main CPU executing **a general purpose instruction set** including instructions from user processes.
    Note that other special purpose processors also exist which perform device specific tasks.
- **Multiprocessor System:** (also parallel systems or tightly coupled systems) Two or more processers in close communication, sharing the computer bus and sometimes the clock, memory and periphery devices.
    Advantages: increase throughput, economy of scale, increase reliability
    **Symmetric Multiprocessing (all CPUs are similar while handling processes) & Asymmetric Multiprocessing** (master monitoring slaves to handle certain processes )
- **Clustered System:** Composed of two or more **individual systems** coupled together
    Can be structured asymmetrically & symmetrically
**System Program**: e.g Operating systems, device drivers, BIOS and other firmware, compilers, debuggers, web servers, database management systems, communication protocols and networking utilities
**Application Program:** e.g Word Processor, Spreadsheets, Compiler, Web browser…
**Operating System(OS):** a system program that manages computer hardware, it acts as an intermediary between computer User and computer Hardware.
**Process:** A program loaded into memory and executing.