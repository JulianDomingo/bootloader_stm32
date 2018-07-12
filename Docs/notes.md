## Side Notes
    * Like RAM, flash memory is R/W, but also non-volatile (writes are saved even when power is shutoff).
        * Flash isn't nearly as fast as RAM (memory)
## Section 2 - Introduction
    * Bootloader is a piece of code which:
        * Acts as the application loader
        * Updates the applications when needed
        * **Stored in flash or ROM (in addition to vector table)**
    * IAP (In Application Programming) vs. ISP (In System Programming)
        * IAP: When ICDP isn't available in a board, **NEEDS** the bootloader to write or update your application binary into flash memory / ROM of board
        * ISP: makes use of in-circuit debugger / programmer (ICDP) to debug program and activate running of bootloader program upon board reset (through switching of boot pins)
    * Bootloader may or may not run whenever board undergoes reset (depends on board)
## Section 3 - MCU Memory, Reset Sequence, and Boot Configs
    * STM32F Memory Organization    
        * Internal Flash Memory
            * Flash memory organization
                * Main memory is divided into sectors
                * Application code stored contiguously starting at specified sector (by default sector 0)
            * Stores: 
                * Application code and its read-only data 
                * Interrupt Vector Table: list of addresses to interrupt handlers
            * Non-volatile: retains data stored when power shut off
        * Internal SRAM1
            * Stores application global data, static variables
            * Used for heap / stack 
            * Volatile: destroyed when power shut off 
            * Processors can execute code stored in SRAM 
        * Internal SRAM2
            * Same as SRAM1 (however, usually smaller in size)
            * SRAM is contiguous - address of SRAM2 starts immediately after end of SRAM1 in memory map 
        * System Memory (ROM)
            * Where bootloader program is stored on board (after tape-out)
            * By default, MCU does **not** execute any code stored here, but can configure to boot / run bootloader if desired
        * OTP (One-time programmable) Memory
            * Stores product / serial number of board   
        * Option Bytes Memory 
        * Backup RAM
            * If power to board is cut off, can backup memory in SRAM into a battery
    * Reset Sequence / Memory Aliasing of MCU 
        * When the board / MCU is reset:
            1. PC (program counter) of processor loaded with address 0x0000_0000
            2. MSP (main stack pointer) is initialized to value @ 0x0000_0000 
            3. Processor stores the address of reset handler into PC (value @ 0x0000_0004)
                * Reset handler handles initializations the programmer specifies, written in C / ASM
            4. main() function is called from within reset handler
        * Memory Aliasing
            * **Specific memory aliasing choices are dependent on microcontroller manufacturer**
            * In Cortex-M processors, user flash (flash memory) is mapped from 0x0800_0000 onwards to 0x0000_0000 onwards.
                * Content starting at 0x0800_0000 is the same as the content starting at 0x0000_0000
                * Example:
                    * When processor requests to read the contents of 0x0800_0004, memory sees the request as a read to 0x0000_0004 
    * Boot Configurations of **STM32F**
        * Boot simply means the microcontroller will execute instructions from selected area of memory
        * 2 boot mode selection pins:
            * **<BOOT0, BOOT1>**:
                * <X, 0>: boot from main memory (flash)
                    * In this case, the 0th memory address is aliased to the base address of flash memory
                * <0, 1>: boot from ROM (system memory)
                    * In this case, the 0th memory address is aliased to the base address of ROM
                * <1, 1>: boot from embedded SRAM
                    * In this case, the 0th memory address is aliased to the base address of RAM
        * **Note**: the base addresses of ROM/SRAM/flash are microcontroller-dependent. Must look at *data sheet* for the microcontroller being to see specifics of memory mapping.                    
## Section 4 - Development Board Information   
    * 

