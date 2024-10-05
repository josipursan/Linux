# Booting  
-once motherboard receives a *power good* signal from PSU after the power on button has been pushed,
CPU is started which then resets any leftover data and sets predefind values in its registers  
-processor now starts working in *real mode* - this means addresses correspond to real locations in memory  
  
-memory segmentation - division of computer's primary memory into smaller sections  
&nbsp;&nbsp;&nbsp;-if, for example, our memory is divided into 16-bit memory chunks, we wouldn't be able to address memory above 64KB  
&nbsp;&nbsp;&nbsp;-therefore an alternate method exists, allowing us to do exactly that  
&nbsp;&nbsp;&nbsp;-an address now consists of a *segment selector* (it has the base address), and the offset from this base address : `Physical address = segment selector*16 + offset`  
  
-CS register - **C**ode **S**egment register is used to address the code segment of the memory, i.e. the location in the memory, where code is stored  
&nbsp;&nbsp;&nbsp;-the processor uses CS segment for access to instructions referenced by IP register  
  
-IP register - **I**nstruction **P**ointer register contains the offset within the code segment of memory  
&nbsp;&nbsp;&nbsp;-CS:IP is used to point to the location (ie. calculating the physical address) of the code in memory  
&nbsp;&nbsp;&nbsp;-IP is also called **program counter** - PC simply holds the memory address of the next instruction to be executed  
  
-**reset vector** - default location a CPU will go to to find the first instruction it will execute after a reset  
  
-when booting, this first instruction will usually be a *jmp* to the BIOS entry point  
-now finally the BIOS is starting  
-BIOS initializes and checks the hardware, and then looks for a bootable device  
-boot order is stored in BIOS  
-when attempting to boot from a hard drive, BIOS tries to find a boot sector  
&nbsp;&nbsp;&nbsp;-on hard drives that are MBR partitioned, the boot sector is stored in the first 446 bytes of the first sector  (the final two bytes of the first sector are 0x55 and 0xaa - this designates to BIOS that the device is bootable)  
-after this, BIOS hand over the control to the **bootloader**  
  
-GRUB 2 will be used as an example to exaplain how bootloaders work  
-so, now that BIOS has chosen a boot device, it transfers control to boot sector code, and **boot.img** execution starts  
  
-**boot.img** - a simple piece of code  
&nbsp;&nbsp;&nbsp;-it has a pointer used to jump to the location of GRUB 2's core image  
&nbsp;&nbsp;&nbsp;-the core image begins **diskboot.img**  
  
-**diskboot.img** - usually stored immediately after the first sector in the unused space before the first partition  
&nbsp;&nbsp;&nbsp;-it loads rest of the core image - contains GRUB 2's kernel and drivers for handling fs  
&nbsp;&nbsp;&nbsp;-then **grub_main** function is executed  
  
-**grub_main** - initialies the console, gets the base address for modules, sets the root device, loads/parses the grub configuration file, loads modules, etc.  
&nbsp;&nbsp;&nbsp;-at the end of its all tasks, **grub_main** moves grub to **normal mode**  
  
-**grub_normal_execute** - completes the final preparations and shows us a menu to choose an OS  
&nbsp;&nbsp;&nbsp;-when user selects which OS to boot, `grub_menu_execute_entry` runs and executes grub `boot` command  
  
-not bootloader (GRUB 2 in our example) must read and fill some field of the kernel setup header (this is a header file like any other)  
&nbsp;&nbsp;&nbsp;-some of these values are `ram_size`, `boot_flag`, etc.  
&nbsp;&nbsp;&nbsp;-the bootloader must fill all of the necessary headers with values received from the command line, or with values calculated during the boot  
  
-once bootloader transfers control to kernel, kernel will start at  
`X + sizeof(KernelBootSector) + 1`  
&nbsp;&nbsp;&nbsp;-`X` is the address of the kernel boot sector being loaded  


TO DO : kernel starting

![Alternative text if path fails](path/to/image/myImage.jpg "Some alt text")