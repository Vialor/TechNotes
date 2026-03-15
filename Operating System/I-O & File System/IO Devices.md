## Block Devices
[[Hard Disk Drives HDD]]
[[Flash-based SSD]]
## Network Device
- **以太网设备**：物理网卡（如 `/dev/eth0`）。
- **Wi-Fi设备**：无线网络适配器（如 `/dev/wlan0`）。
- **虚拟网络接口**：例如 `lo` 是回环接口，`veth` 是虚拟网络接口。
## Character Device
- **键盘**：按键传送字符
- **鼠标**：光标移动和点击事件
- **串口设备**（如 `/dev/ttyS0`）
## Pseudo Device
- **/dev/null**：这是一个虚拟设备，写入的数据会被丢弃（“黑洞”设备），但读取时始终返回EOF。
- **/dev/zero**：提供无限的零字节流，用于初始化内存或其他操作。
- **/dev/random**：生成随机数据流，通常用于加密或安全相关的操作。
- **/dev/urandom**：类似于 `/dev/random`，但提供的是伪随机数据，生成速度更快。
## A **Canonical** Peripheral Device Structure
Interface: Registers: Status, Command, Data
Internals: Micro-controller(CPU), Memory(SRAM, DRAM or both), Other hardware-specific chips
## A **Canonical** Protocol between OS and peripherals
1. Polling the device to wait till it’s not busy
2. Write data to Data register; Write command to Command register
3. Start the device and execute the command
4. Polling the device to wait till it’s not busy, so OS knows the task has been done
**Polling:** waiting for Status change
## Efforts to improve the **Canonical** Protocol
### Interrupt
For slow I/O operations, instead of polling, we context switch to other tasks and switch back when I/O is finished. CPU knows the device has finished by a hardware interrupt.
- Heads-up:
    1. Context switching could be expensive especially when I/O operations are relatively fast.
    2. Network concerns: OS could **livelock** due to a sudden huge stream of incoming packets each generating an interrupt
        **Livelock:** only process interrupts and never allows user-level process to run
- Optimizations:
    1. A hybrid two-phased approach: polling ⇒ context switch
    2. **Coalescing**: a device wait for a while before raising interrupts, thus allows multiple interrupts coalesced into a single interrupt delivery lowering the overhead of interrupt processing.
### Methods of Device Interaction
**PIO Programmed I/O**: a method of data movement between the CPU and peripheral devices
**Port I/O**: OS sends explicit instructions to devices by specify a register with data in it and a port which names the device.
Need to have extra CPU instructions
Not accessible from C/C++ code, only assembly code
**Memory-mapped I/O**: Hardware makes device registers available as if they were memory location. Hardware would routes OS operations to read/write to such registers.  
Certain portions of physical memory are assigned to the I/O devices  
  
**DMA**: Direct Memory Access: a method of data movement between system memory and peripheral devices without main CPU involved
With PIO, CPU has to copy data from memory to the devices explicitly, which takes time. DMA engine is a specific device within the system. It orchestrates data movement memory and devices without much CPU intervention. DMA controller raise an interrupt so that OS know data transfer has finished.
Direct Memory Access (DMA): CPU is fast, devices are slow, don’t want to waste CPU’s time waiting
  
**I/O Programming Model**
	Programmed I/O
	Interrupt-driven I/O
	DMA I/O