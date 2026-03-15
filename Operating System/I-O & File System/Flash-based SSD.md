SSDs generally have better performance than HHDs, but much more expensive.
# Basic Terms
**Flash**闪存 **(NAND-based flash)**: a transistor based storage unit used in memory, processors and SSD
**Solid State Device SSD:** Flash chips**, SRAM (Static Random Access Memory)**, Flash Controller
**Single Level Cell SLC**: use one transistor to store a single bit (MLC stores 2 bits， TLC stores three bits)
**Bank/Plane**: a large number of cells accessed in two different size units: blocks(large, 128KB-2MB), pages(small, 1KB-8KB)
**Basic Flash Operations  
  
**Read a page, Program a page, Erase a block  
expenses: read < program < erase  
**Flash Translation Layer FTL** is a layer within SSD.  
It takes in requests on  
**logical blocks** which are described in device interface, and turn them into low-level operations on the underlying **physical blocks and pages**.
# SSD
## Goal
### Performance
reduce **write amplification**: `traffic FTL issues to flash chips / traffic clients issue to SSD`
### Reliability
**wear out**: When flash blocks are erased and programmed, it becomes more and more difficult to differentiate 1 and 0.
**wear leveling**: try to assure each flash block wear out at the same rate  
1. use log structured FTL  
2. periodically move long-live read-only data from blocks with more wear out to blocks with less wear out.  
**disturbance**: when reading/writing a page, the bits in the neighboring pages that get flipped are called **read/program disturbs**
## **Log Structured FTL**

> **Direct Mapped FTL**: logical page is mapped directly to the physical page
**mapping table**: track the mapping of logic and physical pages  
stored in SRAM  
when shut down, is stored to  
**out-of-band OOB** area, and will be reconstruct after restart
**logging**: writing happens on the next free physical page
  
**Log Structured FTL** leads to 2 questions:
### Garbage Collection GC

> the process of finding **garbage blocks (dead blocks)** and reclaiming them
1. find a block that contains dead blocks  
2. log all the live pages within the block (on the next free pages)  
3. erase the block  
GC leads to write amplification  
use  
**Overprovision** to fight the cost of GC: add extra flash capacity and put GC to the background so it can be done in less busy time
### Reduce Mapping Table Size
1. **Block Level FTL**: store blocks instead of pages in mapping table  
    It leads to write amplification in small writes.  
    
2. **Hybrid Mapping:  
    log table  
    **: a page-mapping table, which is expected to be small and mainly used for writing  
      
    **data table**: a block-mapping table
Three cases of hybrid mapping: switch merge, partial merge, full merge
1. Page Mapping Plus Caching: only store a small mapping of a active pages as cache (will be expensive when miss)