# VM
**Hypervisor**: A software between VM and hardware managing resources for the VM.

**Full virtualization**: The guest OS is unaware that it is running in a virtualized environment. The hypervisor provides an abstraction layer that presents virtual hardware to the guest OS, making it believe it is interacting with physical hardware.  
e.g. VirtualBox, VMWare  
**Paravirtualization**: The guest operating system is modified to be aware that it is running in a virtualized environment. The guest OS communicates with the hypervisor through a set of specialized API calls (hypercalls).  
e.g. Xen, Amazon EC2, Hyper-V (WSL)  
  
**Type1 Hypervisor**: built upon hardware, supports full virtualization and paravirtualization
**Type2 Hypervisor**: built upon the main OS, supports full virtualization
# Container
**Container virtualization**: share the same OS, but use separate binaries/libraries
[[Docker]]