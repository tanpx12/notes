# Intel processor and GNU Assembler

## IA32 architecture
- Start from 80386
- Register 
- Instruction cycle
- Memory management

**Register**
- 8 32-bit general-purpose registers
- 6 16-bit segment registers
- Flag register and instruction register
- EAX: Acummulator register - Automatically used by *multiplication* and *division*
- ECX: Counter register - Automatically used by LOOP
- ESP: Stack Pointer register - PUSH, POP changes values of these registers.
- ESI, EDI: Source index and destination index - Used in operations with string
- EBP: Base address - Refer to local variable in function
- EAX, EBX, ECX, EDX are extended 32-bit registers.
	- AX is a register of 16 low bits of EAX
	- AH, AL is respectively 8 high bits and low bits of AX
- SI, DI, SP, BP is 16 low bits of ESI, EDI, ESP, EBP respectively
- EFLAGS: this register contains a status flag.
- 8 80-bit registers of float ST(0) - ST(7)
- 8 64-bit registers for MMX 
- 8 128-bit XMM register (used for SSE)

**Basic instructions**

**Memory management modes**

**Real address mode**

**Flat address mode**

**GNU assembler**
- Is an assembler towards multiple different architecture such as: Intel IA32, ARM, ...
- Is a component in the package `binutils` and other system component such as `ld`, `ar`
- Input is an assembly program and output is an object program

**Assembly program**
- Assembly program is a form of intermediate code
- Represent symbols for object code.
- Contains the following information
	- Instruction for executing program
	- Data definition
	- Memory segmentation
	- Linking and Debugging information
- Is a sequence of *instruction* and *pseudo-instruction*
	- Instruction: corresponds to instructions of micro-processor, depends on architecture
	- Pseudo-instruction: supports generating object code, depends on the assembler

**Addressing mode**
- Register addressing (prefix %)
- 32 bit: %eax, %ebx, %ecx, %edx, %edi, %esi, %ebp, %esp
- 16 bit: %ax, %bx, %cx, %dx, %di, %si, %bp, %sp
	- 16 low bits of corresponding register
- 8 bit: %ah, %al, %bh, %ch, %cl, %dh, %dl
	- 8 low bits and 8 high bits of corresponding 16-bit register
- Direct value addressing (prefix $)
	- $1
- Absolute/direct addressing 
	- 1
- Relative addressing: -4(%ebp)

**Pseudo-instruction**

`<label> <opcode> <list of parameter>`
- Pseudo-instruction is directives which is used by assembler to execute a certain work in the process of assembling
	- Define data
	- Memory segmentation
	- Define a range and linking directive

**Label and its attribute**

- Label is used as reminder symbol instead of instruction address or data address.
- Type of label
	- `.type <lable>, <label type (function/object)>`
- Size of label
	- `.size <label>, <label size>`
- Range of label
	- Global `.global <label>`
	- Local `.local <label>`

**Data definition**

- Define an integer of 4 bytes 
	- `x : .long 12`
- Define an integer of 2 bytes
	- `x: .word 34`
- Define an integer of 1 byte
	- `x: .byte 5`
	- `c: .byte 'A'`
- Define an array
	- `a: .space 20`
- Define a string 
	- `m1: .ascii "Hello World!\0"`
	- `m2: .asciiz "Hello World!"`

## Memory segmentation

- Area of program code 
	- .text
	- Readable and executable
- Data area
	- Data is initialized
		- Normal data `.data`
		- Readonly data `.section`, `.rodata`
	- Data is not initialized (BBS)
- Stack
	- Data is allocated in execution time. 
		- Parameter for function
		- Return address
		- Local variable
	- Readable, writeable, not executable

## Subroutine

- Is a portion of code within larger program that performs a specific task.
	- Function, procedure
	- Relatively independent of the remaining code
	- Can be reusable
	- Can accept paramenter and return result
- Terms 
	- Caller 
	- Callee
- Active a subroutine
	- Pass parameter
	- Store return address and pass the control to the callee
	- Store local variable of the caller
	- Execute the callee
- Finalize subroutine
	- Pass return result
	- Restore local variable of the caller
	- Go back the caller by using return address stored.

## Stack and stack frame

- Stack frame/activation is a portion of stack that is allocated whenever a subroutine actives.
	- Store values of local variable in the subroutine
	- Is allocated when a subroutine is executed and deallocated when a subroutine returns to its caller
	- Register %ebp holds a pointer to original address of callee's activation record.
	- Original address of activation record of callee are stored on stack and restored before callee return to its caller
- Allocate stack frame: 
``` assembly 
pushl %ebp
movl %esp, %ebp
subl <size>, %esp
enter <size>, 0
```
- Deallocate stack frame: 
``` assembly
movl %ebp, %esp
popl %ebp
leave
```

Calling convention
- To standarlize the interface between caller and callee
	- Parameter passing convention
	- Convention for getting return value
- Function calling convention
	- `cdecl`
	- `pascal`
	- `stdcal`
	- `fastcal`

**Function calling convention `cdecl`**
- Apply for C/C++ on Intel x86 processor
	- Parameters are passed via the stack in the right-to-left order
	- Register %eax is used to store return value
	- Caller plays a role of passing parameter and releasing parameter on the stack
	- Allow passing a undefined amount of parameter (varargs)
- Example:
```
int func(int,int,int);     push c
int a, b, c, d;            push b
				           push a
d = func(a,b,c);           call func
                           add $12, %esp
                           mov %eax, d
```

**Function calling convention `pascal`**
- Apply for application on Windows 3.x and OS/2
	- Parameters are passed via the stack in the left-to-right order
	- Register %eax is used to store return and value
	- Caller plays a role of passing parameter and callee releases a portion of stack that holds parameter
	- Only accept parameters with an defined amount and size.
- Example: 
```
int func(int, int, int);    push a
int a, b, c, d;             push b
					        push c
d = func(a, b, c)           call func
							move %eax, d
```
**Function calling convention `stdcal`**
- Apply for Win32 API (similar to `pascal`)

**Function calling convention `fastcal`**
- Apply for Win32 API 
	- Parameters are partly passed via register, and partly via stack
	- Register %eax is used to store return value
	- Depend on compiler
	- Caller plays a role of passing parameter and callee releases a portion of stack that holds parameter
- Example: 
```
int func(int, int, int);    mov a, %eax
int a,b,c,d;                mov b, %edx
							mov c, %ecx
d = func(a, b, c);          call func
							mov %eax, d
```


# Object file
- Object files are *binary files* that is produced by an assembler or compiler.
- Object files can be combined (by the linker) to generate a single object.
- Object files can be loaded on memory (by the loader) to execute or be used by other programs.
## Classification of object file

**Linkable**
- Linkable with other object files
- Used as input to the linker 
- Contain global symbols and relocation information

**Executable**
- Loadable on memory and executable
- Usually contain page code for easily mapping on address space
- Not contain symbol
- Not have (very little) relocation information

**Loadable**
- Be loadable onto memory and used by other programs (libraries)
- Static linking: Not contain symbol
- Dynamic linking: Contain symbols and relocation information

## Content of an object file

**Header** : General information about the whole object file
- Size of each segment
- Name of the object file
- Creation date
**Object code**  
- Execution code
- Data
**Relocation information**
- Position in object code need to be re-aligned when the linker changes the original address
**Symbol**
- Global symbols are defined in module
- Symbols refer to the outside of module
- Symbols are defined by the linker
**Debug information**
- Content of source file and row index.
- Symbol that are locally used in module
- Data structures are used in object code
**Form of object code**
- MS-DOS COM (Null Object Format)
- MS-DOS EXE (MZ)
- UNIX a.out
- UNIX ELF (Executable and Linking Format)

## Memory management in 8086 processor
- 8086 processor uses 16-bit registers to address for program
	- E.g Instruction pointer IP, Stack pointer SP
	- 64KB memory limit
- To use the memory space greater than 64KB, it needs to combine with 16-bit segment registers
	- Segment register for program code CS
	- Segment register for stack SS
	- Segment register for data DS,ES
- Address conversion
	- (Segment: offset) -> absolute address = (segment Ish 4) + offset
	- Access up to 1 MB of memory

## Memory management in MS-DOS
- All programs are loaded onto the same address space
- When a program is loaded
	- A new memory region is allocated immediately after the memory region was used, on top of freedom memory
	- PSP(Program Segment Prefix, 256 bytes) is created on top of this memory region
		- Contain information about size of the allocated memory region, point to caller's PSP, etc.
	- Load and execute the program
- The program is not loaded at a fixed address.

## The object file MS-DOS COM (Null Object Format)

**Characteristic**
- Simple
- Only contain program code (execution code, data), not contain anything else
- Load and execute (by operating system)
	- Determine the loading address of program
	- Create PSP on memory
	- Load the program starting from the offset 0x0100
	- Initialize segment registers and stack pointer (SP)
	- Execute the program from the address 0x0100
- A program can be loaded on any memory region 
- 64KB memory size limit

## The object file MS-DOS EXE
- Header 
	- Size (header, relocation table, program, data)
	- Initial values for SP and IP
	- `minalloc` stores size of BBS
	- `maxalloc`
- Program and data
	- Be combined together
	- BBS region is particularly determined by `minalloc`
- Relocation table
	- Program loading address is not specified when compiling
	- The compiler generates code for the program starting from the address 0x0000
	- Jump instruction (across the border of a segment) is set by real address (generated by compiler)
	- Read the header and determine size of memory allocating for program
	- Create PSP on memory
	- Load program (size = nblocks * 512 + lastsize)
	- Relocation
		- Position: `relocpos`
		- Amount: `nreloc`
	- Rewrite operands of programs instructions (Especially segment address) described in relocation table.
	- Initialize SP,IP and execute the program

## Memory segments in the file a.out

- Program segment (text): contain program code
- Data segment (data): contain initialized data
- Other segment 
	- Relocation table for program 
	- Relocation table for data
	- Symbol table 
	- Name table

## Load and execution the object file a.out

- Read the header and calculate data size
- Check program code to see if it is shared by other process
	- If so, mapping the shared memory
	- If not, allocate a new memory region for the code
- Prepare the data area that is initialized and not initialized
	- Read and copy the initialized data.
	- Erase the un-initialized data to 0.
- Create stack
- Set value for register and pass the control to the beginning of program.

## Derivative form of the object file a.out

- ZMAGIC: designed for paging mechnism
	- Format is in page (4KB)
	- Operating system can store/restore a raw memory image to disk
- QMAGIC: 
	- Eliminate redundancy in the header and segments
	- To avoid NULL pointer, operating system does not map the page 0

## Relocation form of a.out

- Relocation form of a.out is a form of object file supporting linking feature (file .o)
- Contain the following information: 
	- Relocation information for program code 
	- Relocation information for data
	- Symbol table
	- Name table
- Not support
	- Dynamic linking
	- Object-oriented language

## The object file ELF (executable and linkable format)
- Linkable, executable and loadable
- Support dynamic linking
- Support object-oriented languages
- Header has rich information
	- Architecture
	- Byte order
	- 32/64 bit

# Linker

- Linker is a system software that plays a role of combining multiple objects into a single executable program
	- Input are object files being linkable
	- Output is a new object file
- What does a linker do ?
	- Plan memory for the output object file
	- Symbol management
	- Relocate variables and data based on the above information

**Memory planning**
- Object file is organized into segment for executable code and data.
	- Executable code
	- Data
		- Initialized data
		- Uninitialized data (bbs)
	- Each segment is assigned a base address matching with memory model
		- Object defined in segment is relatively referred to this base address
	- Segments are not allowed to overlap
	- Linker calculates size of each segment of the input object file, pairs segments having the similar role into a single segment, and plans memory for each of these segments on the output object file.

**Segment alignment**

**Memory planning when linking multiple-segment object file**
- Two phase of linking multiple-segment object file
	- Read object as input, determine size of segments
	- Relocate segments in the output file
- Memory alignment
	- For the input segments which belongs to the same output segment, using the method of word alignment
	- Relocate segments in the output file
	- For the input segments which belongs to the same output segment, using the method of word alignment
	- For the different output segments, using the method of page alignment 
	- Data and BBS, using the word alignment because actually they have the same feature.

**Memory planning for ELF**

# Startup process
## Startup an application program

- Operating system loads an application program
	- Create new process
	- Prepare memory space
	- Prepare environment information
	- Move to the entry point of the program `_start` 
- Execute the loaded program 
	- Prepare execution environment `crt1.o`
	- Call the function `_main` 

## Startup an application program for OS
- An application program is executed from a shell
	- Shell (parent process) creates a new process (child process) by using the system call `fork()`.
	- The child process's structure is the same to that of the parent process, but ID is different.
	- The child process calls the fuction `execve()` to load and execute the application program.

**execve()**

`execve(const char *fn, char *argv[], char *env[]`
- Execute the application program `fn` with a list of parameter `argv` and environment variable `env`
	- `fn` is a path to application file
	- `argv` is an array pointing to string which are corresponding to command line paramenter 
	- `env` is an array pointing to strings which are corresponding to environment variables
- Allocate memory for the struct `linux_binprm` describing information about the execution application
- Read `dentry object`, `file object`, `inode object` from the execution file 
- Execute `prepare_binprm()` to fill information in the struct `linux_binprm` 
- Store the execution file name, command line parameter, environment variable onto one or more memory pages
- Execute `search_binary_handler()` to find and load the corresponding execution function `load_binary`
- Check the execution file's magic number
- Read the header of the execution file
- Get the path of application interpreter
- Read `dentry object`,`file object`,`inode object` of the application interpreter
- Check the execution permission and other factors of the application interpreter
- Release resource of the current process `flush_old_exec()`
- Mapping page which stores parameter and environment onto the memory space of the application `setup_arg_pages()`
- Allocate memoru for pages text, data, etc. of the application program `do_mmap()` 
- Load the interpreter `load_elf_interp()`
- Set value `start_code`,`end_code`,...
- Allocate memory for the bss section `do_brk`
- Execute the application `start_thread`

## Execute an application program

- An application program starts from the symbol `_start` (in crt1.c)
	- Prepare to call `main()`
	- Register the called programs when finishing the program `atexit()`
	- Call `_init()` (define in `crtbegin`)
	- Call `main()`
	- Call `exit()` if a control is returned from `main()`



# System call
- In a computer system, applications are normally not allowed to access system resources, instead, they tell the operating system to help them access neccessary resource.
	- System resources are balanced among applications
	- Protect critical data in the system from unwanted changes made by applications
	- Guarantee that an application only operates inside permitted space.
- The OS provides a set of system calls to satisfy the demands of the application that needs to access resources
	- Cover all types of resources 
	- Constructed in a stable and sophisticated manner

## Execution mode

- There are 2 execution modes:
	- User mode: used when executing code in an application
		- Only allowed to access the application's address space.
		- Only permitted to execute a limited set of instructions
		- Forbidden from direct access to data structures that are under protection by the operating system.
	- Kernel mode: used when executing the operating system's instructions.
		- Allowed to execute certain special instructions
		- Allowed to access protected address spaces
		- Allowed to run system programs

## System call and library functions

- System calls are performed in kernel mode, but library functions run in user mode
- System calls that are not linked to applications
- Be limited in number (about 250)
- Library functions are rarely activated with call/return, while system calls make use of software interrupt
- For the ease of use, system call are packed in a wrapper, which will then be used by applications as normal lib function

## System call

- File and I/O operations:
	- `open()`, `close()`: Open and close file
	- `create()`, `unlink()`: Create and delete file
	- `read()`, `write()`: Read and write file
	- `lseek()`: Moving pointer in file
	- `chmod()`: Change file access permissions
	- `mkdir()`,`rmdir()`: Create and delete folder
	- `stat()`: Read information about file status
	- `access()`: Check file access permission
	- `chown()`: Change file owner
	- `ioctl()`: Control devices
- Process management:
	- `fork()`: Create a new process
	- `exec()`: Run an executable file
	- `exit()`: Terminate a process
	- `wait()`: Wait for child process to finish
- Inter-process communication:
	- `kill()`, `sigsend()`: Send a signal to process
	- `pause()`: Suspend a process until receiving a required signal
	- `sigaction()`: Set up procedure to handle signals
- Memory management 
	- `brk()`,`sbrk()`:Change heap size

## Look up system calls
- man 2: look up a system call
- man 3: look up a library 
- man 4: look up a device file
- man 5: look up file format
- man 8: look up command

## Process of making system calls
- Application call routines inside their program
- Application call function in the library (without making system calls)
- System programs call functions in the library (using system calls)
- Library functions activate system calls

## Activate system calls
- Library function (user mode) activate system calls (kernel mode) through interrupts
- A system call is determined with a number and is passed through register `eax`
- Parameters required by the system call are passed through general registers 
	- In case the number of parameter is so large, these parameters are passed through a pointer
	- Pay attention to the parameter
- Variable `errno` stores the error code returned by the system call.
- The interrup handlers (`system_call`) choose the system call from a table of system calls (`sys_call_table`)
	- Store general registers (also parameters of the system call) on kernel stack
	- Choose system call from `sys_call_table` and make a request for the corresponding handler
	- Return control to the library function through instruction `iret`

## Scheduling 
- Some system calls cannot terminate immediately : `nanosleep`,`read`,`write`
- The process need to be scheduled in order to be able to pause (and transfer system control to another process) and wait for success signals from hardware


# Device driver

- Is a kind of system software 
- Control peripherals connecting to a computer (initialize, handle interrupt, exchange data)
- Communicate with users (read, writer, ioctl)
- Device drive in UNIX/Linux:
	- Each device is seen as a file by users
	- Each device has a driver which is built as a kernel module (LKM - Loadable Kernel Module)
- Two main device type 
	- Block device (HDD, CD-ROM,...)
	- Character device (keyboard, modem, printer, ...)

![[Screenshot from 2023-02-23 10-38-16.png]]

## Loadable Kernel Module (LKM)

- Device driver
- File system driver (ext2, ext3, MSDOS FAT32,...)
- System call
- Network devices driver
- tty driver
- Interpreter 

## Device driver 
- Is a module packaging a set of API which is used to communicate with devices
- Hidden hardware operation via file
	- Each device is referred to a file of form of /dev/device
- Coordinate data flows between user and device

# Memory management 
- Memory management on single task OS
	- Programs and OS share the same memory space
	- Only one program can access memory at the same time
- Memory management on multi task OS
	- Multiple process share the same physical memory space
	- Each process can't access the memory of OS and other processes
	- Increase physical memory efficiency

## Memory address mapping on x86

![[Screenshot from 2023-02-23 10-54-03.png]]

## Segmentation

- Divide memory space into segment (text, data, ...)
- Segment table
	- Base: base address of the segment
	- Limit: size of the segment
	- Perm: access permission

**On x86**:
![[Screenshot from 2023-02-23 10-57-26.png]]

## Paging

- Purpose:
	- Reduce memory fragmentation
	- Avoid allocate memory excessively
	- Share memory 
- Memory (virtual and physical) is divided into pages with fix size (4K, 4M)

## Memory protection modes

- Add protection bit for each virtual page
	- present bit: mapping to physical page
	- read/write/execute bits: permission to read/write/execute
	- user bit: access permission in user mode
- MMU check permission every time it access memory.

## Paging in x86-64

- Don't use 64bit address (16EB) only use 48bit
- Page size: 4K, 2M, 1G
- 4 levels
- PAE: 48bit virtual address, 52bit physical address

## Translation Look-aside Buffer - TLB

- Processes only use a little amount of mapping vpn -> ppn (virtual page number -> physical page number)
- To increase the address mapping, associative memory is used for TLB.
- Reduce TLB miss -> Increase TLB size or page size
- Change context 
	- Reload entire TLB (x86)
	- Assign PID for each section on TLB

## Memory sharing

- Multiple process can share the same physical memory page
- Increase usage efficiency of memory
- Exchange information between memory easily

# Virtual file system

## Overview
**Objective**: Allow OS to use different file systems simultaneously
- Disk-like storage file system
- Network file system
- Special file system
**Idea**:
- Using an uniform model for the file systems. Each file system provide specific setting for each component of this uniform model
- Provide a standard access method. Application could access into any file system through system call interfaces, after that an VFS switch will redirect the system call to the setting of the file system.
![[Screenshot from 2023-02-26 22-01-00.png]]

- VFS's job
	- Forward system calls
	- Store metadata of the system call in the memory, combine with page cache
	- Manage uniform access
	- Provide some standard setting like path lookup, file handle management, ...
- FS's job
	- Set up objects/abstract methods of the file system
	- Move standard objects to storage device, using interfaces of block devices.
	- Read/write page file

### Components of a file system
- superblock: contain general information of the file system
- inode: contain metadata of a file (position of the file)
- dentry: contain mapping of the file name and corresponding inode
- file: contain the information of an openning file (dentry and file pointer)

![[Screenshot from 2023-02-26 22-14-28.png]]

**Superblock**
- Contain general information of the file system
- Specific files system set up abstract methods (creat/delete inode)
- Manage synchronize flag of the storage devices
- Kernel manage a list of the used file system

**Inode**
- index node - virtual node: contain metadata of file
	- file properties: access right, size, modified time, ...
	- file content: position where file was store on the disk
	- status flag (ex: dirty inode, dirty data, ...)
- An inode help identify only one file
- Normally store the number of references
	- If # of reference = 0 then inode will be deleted (unlink)
	- Hardlink: new file reference the same inode
- Trick: automatically clean up file when the program has error.
	- Create (1 link)
	- Open (1 link, 1 ref)
	- Unlink

**Dentry**:
- Store the mapping from file name to corresponding inode
	- File name
	- Mapping to inode
	- Mapping to parent dentry (=null if it is the root of the file system)
- Used to be the general data structure of every file system.
- Used to be cached in the memory

**File**
- Store the information of the openning file
	- dentry and file pointer
	- every process manage a table of currently openning file, file handler return from `open()` is the index of this table.
- Usually is the general data structure for every file system.
- Have reference count.
	- `fork`, `copy` file handles and if the child process read this file handle, the file pointer will move.
	- `dup()` will create 2 entry point to the same file structure (increase reference count).


## Typical file systems
### Unix file system
- Bell Labs
- Simple file system
- Problem: slow
	- Small block size
	- Block order of the same file usually disorganized
	- No locality

### Unix fast file system
- BSD Fast File System.
- Increase block size (4096).
- Store bitmap of free block, to easily select the chain of block for the file.
- Store related data near each other (ex: in the same folder), irrelevant data will be distribute far from each other to save the space for related data.

### ext2
- Standard Linux file system
- ext3 = ext2 + journaling
- Use storage layout similar to FFS
- inode contain pointers (32 bits) to blocks
	- direct, indirect, triple indirect
	- biggest file size 4.1TB (4k block)
	- biggest file system size 16TB
![[Screenshot from 2023-02-26 22-43-08.png]]
