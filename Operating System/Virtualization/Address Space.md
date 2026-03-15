# Address Space
**Address Space**: the amount of memory allocated for all possible addresses for a computational entity
    It is a mapping: **Virtual Memory/Address ⇒ Physical Memory/Address**
    Motivations:
        Transparency: OS should implement VM such that it is “invisible” to the programs  
        Efficiency: economic in time and space  
        Protection: prevent a process from accessing memory of other processes or the OS itself 

An simplified address space:  
	(low memory) Program Code, Heap Memory, Free Space(for heap and stack to grow), Stack Memory (high memory)  
	guard page: a special page between heap memory and stack memory to prevent them overwriting each other.

> Heap Memory has nothing to do with the data structure Heap
# Address Translation
## Free List & BitMap
**Free List**: usually a linked list of ranges of physical memory that are not used
**Bitmap**: bit i says whether the ith chunk is free
## Static Relocation (deprecated method)
Use an OS program called **loader** to adjust the address in an executable to desired physical address when the executable is loaded into memory.
downsides: no address protection, difficult to relocate address space later
## Dynamic Relocation / Base and Bounds
Core logic of Address Translation is handled by hardware with no OS intervention. Base and bounds are a pair of registers per CPU to support address translation an bound check.
This method is dynamic because address translation happens at the running time.
**base**: translate address  
physical address = virtual address + base  
**bound(limit)**: ensure the addresses are within address space; raise exceptions otherwise  
Bound can be the size of address space or the physical address of the end of address space. We usually assume the former method.  
**Memory Management Unit MMU**: a part of processer (hence a hardware) that helps address translation

| Event                                 | OS kernel mode                                                                                                                                                                                                | Hardware                                                                                                                                            | Processes user mode                                                 |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| start process A                       | 1  <br>allocate entry in process table  <br>allocate memory for A  <br>set base/bound registers  <br>return-from-trap to A                                                                                    | 2  <br>restore registers of A  <br>switch to user mode  <br>jump to A’s initial PC                                                                  | 3  <br>process A runs                                               |
| A executes load/store instruction     |                                                                                                                                                                                                               | 2  <br>translate virtual address and perform fetch  <br>4  <br>make sure the address is legal  <br>translate virtual address and perform load/store | 1  <br>fetch instruction  <br>3  <br>execute load/store instruction |
| timer interrupts A and switch context | 2  <br>decide: stop A run B  <br>call switch routine:  <br>save regs(A) to proc-struct(A) (including base/bounds)  <br>restore regs(B) from proc-struct(B) (including base/bounds)  <br>return-from-trap to B | 1  <br>move to kernel mode  <br>jump to handler  <br>3  <br>restore registers of B  <br>move to user mode  <br>jump to B’s PC                       | 4  <br>process B runs                                               |
| B has a bad load                      | 3  <br>decide to kill B  <br>deallocate B’s memory  <br>free B’s entry in process table                                                                                                                       | 2  <br>Load is out-of-bounds  <br>move to kernel mode  <br>jump to trap handler                                                                     | 1  <br>B executes a bad load                                        |