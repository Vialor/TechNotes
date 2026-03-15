# Faster Translations
**Translation-Lookaside Buffer TLB**: a hardware cache of popular virtual-to-physical address translation. TLB is a part of MMU, which is on chip.
TLB is based on two premises:  
  
**Temporal Locality:** recently accessed data will be accessed again  
  
**Spatial Locality:** the neighbors of recently accessed data will be accessed
## TLB Issues
### Who should handle TLB misses?
**Hardware-Managed TLB**
Use a page-table base register, and know the exact format of the page tables
e.g. Intel x86
**Software-Managed TLB (modern solution)**
Raise an exception so that OS will look up translation in page table and update TLB with a special privileged instruction. After that OS retries the instruction.
### TLB in a context switch
**background**
TLB only stores page translation of the **current** process for now. Entries of TLB has a valid bit, which simply means whether the translation is valid. (Unlike valid bits in page table, which just means the page has not been allocated)
**solutions**
1. Flush the valid bits in a context switch.
2. Add Address Space Identifier ASID in TLB to differentiate different processes.
### Replacement policy
LRU Least-Recently-Used
Random
### Exceeding TLB Coverage
Exceeding **TLB Coverage** can cause severe performance issue. One Solution is to include support for large page sizes for key data structures, especially for DBMS.
# Smaller Tables
unallocated pages take up too much space in a page table
## Hybrid of segmentation and paging
virtual address: seg, VPN, Offset
segment base: start of page table  
segment bounds:  
**value of maximum valid page in the segment**
```Python
SN: segment bits
PTEAddr = Base[SN] + VPN * sizeof(PTE n)
```
Don’t use this. It inherits all the downsides of segmentation: presumptions on the usage of address space and external fragmentation
## Multi-level Page Table (Modern Solution)
paging of paging
**page directory**: a per process table that stores the location of a page of a page table or whether the page of a page table has no valid pages
**page directory entry PDE**: valid bit, **page frame number PFN**
**page directory index**: page number for the pages of page table, can be extract from choosing the some most significant bits from VPN
More than two levels of indirections could be added to this solution in case that page directory itself gets too large. And thus VPN now can be divided into: PD index 0, PD index 1…, page table index
## Inverted Page Tables IPT
an extreme space saving method
A table for all physical pages. It maps a physical page to the process using it and the corresponding virtual page.
## Swap to Disk
place page tables too large in **kernel virtual memory**, thereby allow swapping those page tables to disk