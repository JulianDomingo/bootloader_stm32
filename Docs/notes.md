* Section 2 - Introduction
    * Bootloader is a piece of code which:
        * Acts as the application loader
        * Updates the applications when needed
        * Stored in flash or ROM
    * IAP (In Application Programming) vs. ISP (In System Programming)
        * IAP: When ICDP isn't available in a board, *NEEDS* the bootloader to write or update your application binary into flash memory / ROM of board
        * ISP: makes use of in-circuit debugger / programmer (ICDP) to debug program and activate running of bootloader program upon board reset (through switching of boot pins)
    * Bootloader may or may not run whenever board undergoes reset (depends on board)
* Section 3 - MCU Memory, Reset Sequence, and Boot Configs
    * TODO

