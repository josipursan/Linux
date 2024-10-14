# First steps in the kernel setup

-this section will cover a lot of new info :  
&nbsp;&nbsp;&nbsp;-what is *protected mode*  
&nbsp;&nbsp;&nbsp;-how we reach it  
&nbsp;&nbsp;&nbsp;-heap initialization  
&nbsp;&nbsp;&nbsp;-console initialization  
&nbsp;&nbsp;&nbsp;-a lot more  
  
## Protected mode  
-also called **protected virtual address mode**  
-it is an operational mode of x86 compatible CPUs  
-it allows system software to use features such as segmentation, virtual memory, paging, etc.  
-protected mode can be entered only after the system software sets up one descriptor table and enables the *Protection Enabled* **(PE)** bit in CR0 (*Control Register 0*)  
  
-this section will cover segmentation - paging will be covered in later sections  
  
-as previously stated, memory addresses consist of two parts in real mode :  
&nbsp;&nbsp;&nbsp;-base address of the segment  
&nbsp;&nbsp;&nbsp;-offset from the segment base  
  
-memory management is completely redone in protected mode  
-there are NO more 64Kb fixed-size segments  
-size and location of each segment is described by an associated data structure called the **Segment descriptor**  
-segment descriptor are stored in a data structure called the **Global Descriptor Table** (GDT)  
  
-GDT is a structure stored in memory, in a specific register called **GDTR**  
-GDTR is a 48 bit register consisting of two parts : size of the global descriptor table (16-bits), and the address of the global descriptor table (32-bits)  
-GDT contains segment descriptors which describe memory segments, and each descriptor is 64-bits in size  
  
*Comment : to be honest, this whole section detailing what each flag in the descriptor means, and how they interact, and what their end effect/meaning is felt a bit too much, which is why I haven't taken any notes for this section. It is always available online for reference. What a mess...*
  
  
-segment registers have *segment selectors*  
-each segment selector is a 16-bit structure comprised of the following elements :  
&nbsp;&nbsp;&nbsp;-**Index** - stores the index number of a descriptor in GDT  
&nbsp;&nbsp;&nbsp;-**TI** (table indicator) - indicates where to search for the descriptor; if 0, then search in GDT; if 1, then search in LDT (Local Descriptor Table)  
&nbsp;&nbsp;&nbsp;-**RPL** - data indicating Privilege Level of the Requester (Requester's Privilege Level)  
  
TO DO : *"The following steps are needed to get a physical address in protected mode:"*