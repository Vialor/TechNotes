> **LDE**: put limits on processes’ ability to run directly on the CPU

**Interrupt**: this is how CPU switch from user mode to kernel mode
**internal/software/synchronous interrupt** (**exception**):  from CPU, related to current instructions  
	trap(sys call), fault(kernel can fix: page not in memory, fetch it), abort(kernel cannot fix, will not return CPU to the process)  
**external/hardware/asynchronous interrupt** (**interrupt** in a narrow sense; this is the most used definition):  from outside of CPU, unrelated to current instructions  
	timer interrupt, IO interrupt  
# Problem 1: We want code to do some restricted operations without giving it complete access to all hardware resources.
**Processor mode:**
**User mode:** a mode where code is restricted in what it can do
**Kernel mode:** a mode where code has access to all the restricted operations
**Context Switch:** CPU switch from one process to another while ensuring tasks do not conflict
(by saving registers of current process and restoring registers of the next process)
- **System Call & Procedure Call:** privileged OS actions (low level, use kernel space) & user actions (high level, use user space)
    
    Programs can make system calls to access some restricted operations.
    
- Main types of System Calls:
    - Process Control
        end, abort
        load, execute
        create processes
        get/set process attributes
        wait for time
        wait/signal event
        allocate/free memory
    - File Manipulation
        create/delete files
        open/close files
        read/write/reposition files
        get/set file attributes
        logically attach/detach devices
    - Device Management
        request/release devices
        read/write/reposition devices
        get/set devices attributes
    - Information Maintenance
        get/set system data
        get/set time/date
        get/set process/file/device attributes
    - Process Communications
        create/delete communication connection
        send/receive messages
        transfer status information
        attach/detach remote devices
**trap table:** initialized by kernel at boot time (initialization itself is privileged operation for OS): OS informs the hardware of the locations of **trap handlers**, so that when system call, the hardware knows what code to jump to.
**trap & return-from-trap:** special instructions to **trap** into the kernel mode, and after finishing the system call work, **return-from-trap** back to user mode
**kernel stack:** After calling **trap**, the hardware saves the registers of the user-mode process onto a **per-process** **kernel stack**, and restores the registers after **return-from-trap**. We need this to resume interrupted processes after system calls.

> `exit()` is a system call which traps into the OS when the process is finished.
# Problem 2: We want OS to regain control of the CPU so that we can switch between processes.
**Cooperative approach**: Processes are trusted to make system call to return control back to the OS.
**Non-cooperative approach**: Use a timer interrupt to force processes to return the control. We call it **Interrupt Timer**.