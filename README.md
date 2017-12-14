# hp-41_CL
## Tools for the HP-41CL by Systemyde (http://www.systemyde.com/hp41/index.html)

These tools help managing your HP-41CL system.

### BACKUP

This program backs up all you need to various media. Dependencies are the YFNX, Power-CL, CLILUP and Lib4 modules.

The program "FLASH" relies on PC41 (https://github.com/isene/pc41) for backup to your PC.

Upon execution, the program first shows the short form of labels A-E and then a-e as prompts:

**__R X H__**

**__M:R,X P:R,H RST__**

Label (menu)    |Description
----------------|-----------
LBL A (R) |Backup RAM (block 800) to Flash block 1FE and MMU (804) to Flash block 1FF
LBL B (X) |Backup eXtended Memory to Flash block 1FD
LBL C (H) |Backup HEPAX RAM in blocks 828 and 829 to Flash (blocks 1F8 and 1F9 respectively)
LBL a (M:R) |Backup RAM (block 800) to mass media (like the cassette drive)
LBL b (M:X) |Backup eXtended Memory to mass media (like the cassette drive)
LBL c (P:R) |Backup RAM (block 800) to your PC via HP-IL (PILbox) and the CLILUP module
LBL d (P:H) |Backup HEPAX RAM in blocks 828 and 829 to your PC via HP-IL (PILbox) and the CLILUP module
LBL e (RST) |Switch to the RESTORE program (see below)

The program relies on the last 8 Flash block being erased idependently (check your HP-41CL manuals to verify that you have the right CL board that treats the last 8 pages individually).

The last 8 Flash blocks will have this setup:

Flash block |Content
------------|-------
1F8 |HEPAX1
1F9 |HEPAX2
1FD |XM
1FE |RAM
1FF |MMU

### RESTORE

This program is the counterpart to the above BACKUP program. Dependencies are the YFNX, Power-CL and Lib4 modules.

The program relies on PC41 (https://github.com/isene/pc41) for backup to your PC.

Upon execution, the program first shows the short form of labels A-E and then a-e as prompts:

**__M R X H FIXR__**

**__M:R,X P:R,H BU__**

Label (menu)    |Description
----------------|-----------
LBL A (M) |Restore MMU setup (change to reflect your own preferences)
LBL B (R) |Restore RAM (block 800) from Flash block 1FE and MMU (804) from Flash block 1FF
LBL C (X) |Restore eXtended Memory from Flash block 1FD 
LBL D (H) |Restore HEPAX RAM (blocks 828 and 829) from Flash (blocks 1F8 and 1F9) AND plugs them to pages #C and #D
LBL E (FIXR) |Restore HEPAX RAM (blocks 828 and 829) from Flash (blocks 1F8 and 1F9) (also 1FC to 81A)
LBL a (M:R) |Restore RAM (block 800) from mass media (like the cassette drive)
LBL b (M:X) |Restore eXtended Memory from mass media (like the cassette drive)
LBL c (P:R) |Restore RAM (block 800) from your PC via HP-IL (PILbox) and the CLILUP module
LBL d (P:H) |Restore HEPAX RAM into blocks 828 and 829 from your PC via HP-IL (PILbox) and the CLILUP module
LBL e (BU) |Switch to the BACKUP program (see abow)

### FLASH

This program makes for easier upgrades of Flash pages in you HP-41CL via the built-in serial port. Dependencies are the YFNF, YFNX, Power-CL and Lib4 modules - which means you cannot use the FLASH program to update these modules in the 41CL flash memory..

Upon execution, the program first shows the short form of labels A-E and then a-e as prompts:

**__INI F R + ↑__**

**__ERS W W8 F/R__**

Label (menu)    |Description
----------------|-----------
LBL A (INI) |Initializes (prepares for data transfer, setting Baud rate to 4800, Turbo to 50x, RAM page to 810, asks for Flash page)
LBL B (F) |Sets the Flash page (if another page is needed than initialized)
LBL C (R) |Sets the RAM page (if another page is needed than initialized)
LBL D (+) |Increments the RAM page by one
LBL E (↑) |Executes YIMP
LBL a (ERS) |Does YFERASE on the Flash page set
LBL b (W) |Does YFWR from the RAM page set to the Flash page set
LBL c (W8) |Does YFWR8 from the RAM page set to the Flash page set
LBL d (F/R) |Displays the current set Flash and RAM pages
LBL e |Go back to showing the program menu

Example: To update sector 1D0 of your HP-41CL, start by XEQ"FLASH". The program asks for the Flash page (enter 1D0) and press R/S.

Then the program asks if you want to copy the 8 pages starting from the flash address you just entered to RAM pages 810-817. Press Y for "Yes" and it will ask you to press SST to commence the copying. The reason I stop the program for you to manually press SST is that several of the built-in functions give feedback on progress only if the functions are entered manually. To skip the copying of the 8 flash pages, press N for "No".

The program is then initialized, showing "810000-0FFF" in the display.

You are now ready to transfer the first image into RAM page 810. Fire up PC41 on your PC with the right image to transfer (pc41 -w TIDES.ROM). 

The PC41 program will tell you to press YIMP first and then ENTER on your PC within a second to transfer the image. Press "E" on your HP-41 (LBL E executes the YIMP) and then ENTER on your PC. The image transfers. You now press "D" to increment the RAM page. It shows "811000-0FFF" indicating it is ready to receive the next page. On your PC, you type "pc41 -w PORTSL.ROM" to transfer the next image. Press "E", then ENTER on your PC, etc.

Upon doing a write to Flash (labels "b" or "c"), you can press R/S and the program will commence with checking if the image matches the CRC value of the FLDB database. A "MATCH" or "DIFF" will be displayed. Pressing R/S again will do the CRC check on the next Flash page, etc. You can also press "F" to do a CRC check on the currently set Flash page at any time.

Do continue uploading images after the first block of images, press "A" (INI) to start afresh.
