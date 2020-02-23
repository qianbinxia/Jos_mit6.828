# Jos_mit6.828 lab1
1. BIOS is "hard-wired" to the physical address range 0x000f0000-0x000fffff to ensures that the BIOS always gets control of the machine first after power cycle.
2. On processor reset, the processor enters real mode and sets CS to 0xf000 and the IP to 0xfff0, so that execution begins at that (CS:IP) segment address.
3. How segment address translated to physical address: physical address = 16 * segment + offset.
4. When the BIOS runs, it sets up an interrupt descriptor table and initializes various devices like VGA display, and search for a bootable device, reads the boot loader from the disk and transfers control to it.
5. Hard Disks for PCs:
    - Divided into 512 byte regions called sectors.
    - The first sector is called boot sector containing the boot loader code.
    - BIOS load the 512-byte boot sector into memory at physical addresses 0x7c00 through 0x7dff, then uses a jmp instruction to set the CS:IP to 0000:7c00, passing control to the boot loader.6. Boot loader:
    - Switches the processor from real mode to 32-bit protected mode to access all the memory above 1MB in the processor's physical address space.
    - Reads the kernel from the hard disk by directly accessing the IDE disk device registers via the x86's special I/O instructions.
7. Real Mode vs Protected Mode
    - Real Mode:
        -> In real mode, memory is limited to only one megabyte (2^20 bytes) with valid range (0x00000 to 0xFFFFF).
        -> a 20-bit number will not fit into any of 8086's 16-bit registers.
        -> Using two 16-bit values to determine an address. The first 16-bit = selector (CS), the second 16-bit = offset (IP).
    - Disadvantages of real mode:
        -> A single selector value can only reference 64K of memory, a program will be splited if larger than 64K, which makes the CS changing when running.
        -> Each byte in memory does not have a unique segmented address.
    - 16-bit protected mode:
        -> In real mode, a selector value is a paragraph number of physical memory.
        -> In protected mode, a selector value is an index into a decriptor table.
        -> In real mode, these segments are at fixed positions in physical memory and the selector denotes the paragraph number of the begning of the segment.
        -> In protected mode, the segments are not at fixed positions in physical memory and in fact, they do not have to be in memory at all.
        -> Protected mode uses a technique called virtual memory, which only keep the data and code currently be using in memory, other data and code are stored on disk.
        -> In 16-bit protected mode, segments are moved between memory nad disk as needed.
        -> In protected mode, each segment is assigned an entry in a descriptor table, which contains the following info:
            - is it currently in memory.
            - if in memory, where is it.
            - access permissions (e.g., read only)
            - the index of the entry of the segment in the selector value stored in segment registers (CS).
        -> One big disadvantage of 16-bit protected mode is that offsets are still 16-bit quantities.
    - 32-bit protected mode:
        -> Two major differences between 386 32-bit and 286 16-bit protected modes:
            - Offsets are expanded to be 32-bits (up tp 4 GB)
            - Segments can be divided into smaller 4K-sized units called pages. The virtual memory system works with pages now instead of segments.
            - This means that only parts of segment may be in memory at any one time.
            - In 286 16-bit mode, either the entire segment is in the memory or none of it is.
8. Operating system kernels often like to be linked and run at very high virtual address, such as 0xf0100000, in order to leave the lower part of the processor's virtual address space for
   user programs to use.
9. In an x86 processor, the call instruction pushes the return address on stack and transfers control to the function.
10. esp pointer to the top avaible element of the stack not the next empty stack position.
