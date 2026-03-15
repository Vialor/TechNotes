# Segmentation
**segment**: a **contiguous** portion of address space of variable length
## Address Translation
Address is specified with **segment number** and **offset**. An illegal address hence leads to **segmentation fault** (for example, offset larger than size in segment table).
offset = virtual address - virtual address of start of the segment  
physical address = base + offset (base is per segment which is determined by checking  
**segment number** against **segment table**)
## Segment Table
**Segment Table**(Segment registers) is stored in MMU (per process) containing following entries:  
segment, base, size,  
**protection bits** and **a bit to support negative growth** like stack.  
With  
**protection bits** specifying permission to read/write, segments can be shared among processes, which is not true for pages.
# Paging
**page**: a **contiguous** portion of address space of **fixed** length
**page frame: Page frame is** a physical property of the main memory; physical memory is organized into page frames. **Page** is a virtual concept in address space; the size of page matches a page frame.
## Address Translation
Address is specified with **Virtual Page Number VPN** and **offset**.
**PPN physical page number**
physical address = PPN, offset (**PPN** is determined by checking **VPN** against **page table**)
## Page Table
**Page Table** is much larger than segment table, and hence has to be stored in memory. Page table is also per process.
**Page Table Entry PTE:**
PFN
protections bits: wether page could be read from, written to, executed from
present bit: wether the page is in physical memory or on disk  
valid bit: wether the page translation is valid(exists in physical memory or disk)  
dirty bit: wether page has been modified since loaded to memory  
reference bit (accessed bit): wether page has been accessed since loaded to memory  
### Find PTE
`PTEAddr = PageTableBaseRegister + VPN * sizeof(PTE)`
# Summary
> page is fixed-sized, segment is variable-sized

**internal fragmentation**: unused space inside the address space of a process  
**external fragmentation**: unused space among the address spaces of processes
**Segmentation** solves internal fragmentation, but has external fragmentation issue and relies on assumptions on the address space usage. **Paging** solves external fragmentation, but has internal fragmentation issue.


```
def findDup(arr):
    i = 0
    while i < len(arr):
        if arr[i] == i+1 or arr[arr[i]-1] == arr[i]:
            i += 1
        else:
            arr[arr[i]-1], arr[i] = arr[i], arr[arr[i]-1]     
    ans = set()
    for i in range(len(arr)):
        if arr[i] != i+1:
            ans.add(arr[i])
    return ans
    
for val in arr:
    arr[i] = val + 1
    del arr[i]
```