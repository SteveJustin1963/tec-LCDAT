# tec-DAT
A TEC-1 Digital Audio Tape data system and LCD display, for saving or playback of programs and displaying 16x2 ASCII characters.  
By Jim Robertson

## From TE-15 

SENDING IN PROGRAMS VIA TAPE 

We are looking forward to readers sending programs to us. ... Provide us with a copy of the program. Save it on tape with a crystal speed of half 3.58MHz. ... We also need documentation on the program. Write what it does and where it runs in memory and include any notes you may have generated. The first thing we will do is disassemble it and load it into our IBM clone. Here we can format it for publishing. For the sake of our disassembler, please, if you can, put tables at the end of the program code and write down where the tables are located. This way we can use our HEX dump routine and tack the tables on at the end of the code.

JMON UP-GRADES (parts omitted, see full article)

JMON has been designed to be upgraded without losing software compatibility. Some likely changes are the removal of the low speed tape save (unless there is a storm of protest). This will decrease the software overhead in the tone routine and even-up the period measurement. The result will be an increase in the tolerance of different TEC frequencies and different tape speeds. This should make it possible to freely interchange half 3.58MHz and half 4.MHz tape software as well as allowing poorer quality tape players to be used. The single stepper, which has no effect on the MONitor at all, may be shifted to a more specialized ROM to increase the stepper's abilities.- The keyboard and LCD RST's will not be changed, so any routine you write using these will run on future up-grades. The same cannot be said if you directly call into JMON. So don't do it! 

![](https://github.com/SteveJustin1963/tec-DAT/blob/master/dat-15-17.png)

THE TAPE SOFTWARE

The TAPE SAVE facility is versatile and reliable. Some of the functions include: 
* 300 and 600 baud tape SAVE, 
* auto execution, * LOAD selected file, 
* LOAD next file, * LOAD at optional address, 
* TEST tape to memory block 
* TEST tape check sum. 

Both tests may be combined with other options. The tape software uses the universal MENU driver and perimeter handler. These routines allow easy selection of cassette functions (e.g. Load, Save, etc.) and easy passing of variables to the tape software. The tape software contains check-sum error detection that allows the user to know if the load has failed. A checksum compare is performed after every page (256 bytes) and also after the leader is loaded. This means the user does not need to wait until the end of a load or test for error detection. Each full page to be loaded, tested or saved, is displayed on the TEC LED display. Up to 16 pages are displayed. Upon completion of a tape operation, the MENU is re-entered with an appropriate display showing:
* -END -S (END SAVE); 
* PASS CS (CHECK SUM); 
* PASS Tb (TEST BLOCK); 
* PASS LA (LOAD); 
* FAIL CS (CHECK SUM); 
* FAIL Tb (TEST BLOCK); 
* FAIL Ld (LOAD). 

The one exception is when an auto execute is performed after a successful load. The tape software will display each file as it is found and also echo the tape signal.

from page 22...

THE TAPE SYSTEM / TEC CONSIDERATIONS

The tape software works on any type of TEC, the only consideration is the various different clock speeds. The following description generally applies to TEC's with a crystal oscillator that is fitted with a colour burst (3.58MHz) crystal and divide-by-two stage. If you are still using a 4049 based oscillator, the tape system will work ok, but it will be very important to note the TEC clock speed when saving as the TEC must be set to the same speed when re-loading. Another problem can be the drift in frequency over a temperature range and the different oscillator frequencies between TEC's. When saving a tape, the best idea is to wind the clock up to full speed, and then turn back the speed control pot one quarter of a turn. This will allow you compensate for speed drift if ever required. The tape also works very reliably with a 4MHz crystal and divide by two stage, however a tape written using a 3.58MHz oscillator cannot be loaded by a TEC that uses a 4MHz oscillator, and vice versa. If you are sending programs into TE on tape, they must be recorded with the 3.58MHz crystal. (divided by two). The tape system has been extensively tested and found to be very reliable under a wide range of conditions. We don't expect you to have any trouble in getting it to work reliably for yourself. 

To start with, you need a JMON monitor ROM as the tape software is inside this ROM. Secondly, you will need a cassette recorder with both "mic" and "ear" sockets. Any audio cassette player of reasonable quality should be ok, provided it has the two sockets mentioned above. We have tested more than six types, and found them to be quite suitable. Thirdly, you will need to have constructed the cassette interface on the LCD interface board and have made up the two connecting cables, with 3.5mm plugs on each end. Finally you will need a new C60 or C90 cassette of the better quality types, such as TDK or Sony. We found the cheap tapes from the junk shops or supermarkets to be unreliable. (Some of them didn't work AT ALL, so don't take the chance). Now connect the "mic" on the tape recorder to the "tape out" from the TECand "ear" socket to the "tape-in" on the TEC. (It's a good idea to mark the cables between the recorder and the TEC to prevent incorrectly connecting the leads). Insert a tape and we are ready to learn how to operate the system. 

HOW TO OPERATE THE TAPE SYSTEM.

We will start by saving a few bytes at 0900. 

Enter at 0900 the following: 

01 02 03 04 05 06 07 08 09 0A. OK. 

Now connect up the tape recorder as described above and call up the tape software by pushing shift and zero at the same time or Address, "+","0" consecutively. The TEC display will now show "SAVE-H" and this is the heading for SAVE at HIGH SPEED. Now select this by hitting "GO" The display will now have a random two-byte value in the address display and "-F' in the data display. The "-F' in the data display is for the file number, while the address number is just junk from the RAM. You can enter a file number by pressing the data keys. Enter anything you want. The numbers you enter will shift across in the same fashion as when entering an address on the MONitor. Then when you have entered a file number, press the "+" key. The data display will now show "-S". This is where you enter the start of the block you wish to output. Enter 0900, and then press "+". The data displays now show "-E." This is where you enter the address of the last byte of the block to be saved. Enter 090A and press "+". The next data display is "-G". This is the OPTIONAL AUTO-GO address, this is always set to FFFF by the software as this is its NON-ACTIVE state, i.e. NO AUTO-GO upon a re-load. ANY other value entered here will result in an automatic execution upon a SUCCESSFUL LOAD AT THE ADDRESS ENTERED HERE. We don't require auto execution, so leave this at FFFF. Now press play and record on the tape recorder and wait for the clear plastic leader to pass if at the start of a tape. Incidentally, it is not a good idea to remove this leader as has been advised in another magazine, as it protects the tape from stretching and possibly breaking, when rewound. When the tape is right, press GO. The display will blank and a continuous tone will be heard from the speaker. After a few seconds the file information will be outputted and then a period of high frequency tone. This "middle sync" tone is to cover the time that the filename is displayed when re-loading.   

After the high tone, the code will be outputted and also a digit will appear on the TEC LED display. This is the number of COMPLETE pages to be saved. In this case it will be zero. A point to raise here is that if you ever accidentally enter a start address that is HIGHER than the end address, when GO is pressed, the software will detect this and display "Err-In". In this case, Push "+" or "-" to go back to the perimeter handler where you can correct the error. When the code has been saved, a short end tone will be heard and then the menu will re-appear with "-END-S", meaning end of save. Once the code has been saved, rewind the tape. To re-load the tape press the "+" key and you will see "SAVE-L" on the display, then "TEST-BL", "TEST-CS", then you will come to "LOAD-T" (for load tape). Note that there is no "TESTH" or "TEST-L" for low and high speeds as the test and load routine will load either speed automatically. Press GO. The data display shows "- F' for file number. This will be as you left it when you saved. When loading or testing from tape, the file number here determines which file will be subject to the selected operation. If you enter here, the next file found will be used, regardless of its file number. For now, we will leave it as it is. Next push "+". The data display will show "-S", meaning Start address. This is always set to H4114by the software. The start address allows you to optionally load a file or test a file at an address different to the one on the tape, (which is the address from which it was saved). To demonstrate its operation and to make it a more convincing trial, we will enter 0A00. The file will now be loaded at 0A00. If you press the "+" key again, you will be back at the file name. (This last point demonstrates the programmable number of "windows" feature of the perimeter handler. It was set up for 2 "windows" by a short routine entered from the Menu driver before passing control to the Perimeter handler, remember that there was 4 "windows" when you saved the file). Now press GO. The display will blank. Now start the tape playing. The sound from the tape will be echoed on the TEC speaker. Soon the leader will be heard and it should sound as crisp as when it was saved. If not, experiment with the volume. The interface allows for a wide variation of volumes but 3/4 volume is a good place to start. After the leader has passed, the file name is loaded and should appear on the display. If it was not correctly loaded, "FAIL-Ld" will appear. In this case experiment with the volume and retry. After a few seconds the file name disappears and the number of complete pages to load are displayed on the middle digit. The code is now being loaded. The code is loaded very quickly and hopefully a "PASS-Ld" will appear. If not, re-try with a different volume setting. After you have successfully loaded, hit reset and ADdress 0A00 and 01, 02 etc. will be found. If you are unable to get a successful load after many attempts, then skip ahead to the trouble shooting section. Now we have a successful load, we will experiment with the TEST BLOCK function. Change a byte in the 0A00 block. Now call up the tape software (Shift-0, or ADdress, "+" ,0), select "TEST-BL", and LEAVE F1-41+ at the optional start. ("-S") Then rewind the tape and play it back like you did when loading. At the end of the test, the display comes back with "PASS-TB". Now do this again, but this time enter 0A00 at the optional start and F141-it. for the file. This will demonstrate the load/test next file feature. Because 0A00 has been entered in the optional start "window", the test will be between tape and the code at 0A00. Rewind the tape and press "GO" on the TEC, then play the tape. Because a byte has been changed, the test this time will fail and the display will show "FAIL-TB." Use the test-block feature whenever you wish to compare a tape file with a memory block or test that a save operation was successful. If ever revising software on a tape of which you do not have a copy in memory, use the test checksum (TESTCS) to ensure that the file is good. By use of the "LOAD NEXT FILE" feature (F1-14F in the file number window) you can go through a tape completely, checking each file. THE "AUTO-GO" To use the Auto-GO feature, you must enter the required GO address WHEN YOU SAVE THE FILE. The go address is entered under the "-G" data display.
Experiment with the following: 

0900: 21 10 09 11 00 08 01 06

0908: 00 ED B0 CD 36 08 18 FB

0910: 6F EA C6 EB E3 EB.

Save this as described above, but this time enter 0900 under the "-S" heading, 0921 under "-E", and 0900 under "-G". Now re-load it and if the load is successful, the program will start automatically and an appropriate display message will appear. 

USING THE TAPE SYSTEM. 

The primary use for your tape save system is as a mass storage device for your files. Files may be saved and loaded as described previously, the important addition here is good paper-work habits. It is very important to keep a log of your files or you will quickly forget what you have, where it is located, and you will end up writing over your files! Your log system must include identifying each cassette and the side of the tape, the files on the cassette in the correct order, how many of each file, the date and any notes on the file. If your recorder has a tape counter facility, it makes good practice to record the readings from this, so that files may be quickly found anywhere on the tape. Also a great aid is to log approximately the location of each file e.g. half-way, 30 seconds from rewind from end etc. Apply the above idea to the start of vacant area on the tape also. Another very good way to use the tape system is as a "RUNNING LOG", where a whole side of a cassette is used to save a developing program, stage-by-stage. If you crash your program, you can re-load it back from tape. A good idea here is to use the high byte of the file number as the program identification and the low byte as the progressive count or version number on the tape. When you have a final version, then save that on a permanent cassette. The "RUNNING LOG" cassette can then be used over and over. Once again, paper work is very important. Make sure you document any differences between successive files. This may help later in de-bugging. Also, always include the date and time as this will give a chronological order to your work.  If you are wondering how many times you should save a file, and at what speed, the answer really depends on the reliability of your system. The major factors in reliability are your tape player and cassette quality and how well you constructed your interface. If any of these are borderline, the system may work but you may have a higher than normal failure rate. Our tests show reliability at better than 98% on saves of 2K blocks. Different cassettes and players were used over many months and rarely did a fault creep in. You can test your system out by saving the monitor 10 times on each speed and then perform a BLOCK TEST. You should get at the very least, 17 out of 20 passes. If not, some trouble-shooting may be required. If you get 19 or 20 you could probably get away with high speed saves and not have to worry about checking them on your running log. For permanent storage, a good system is a high speed save, then two low speed saves and check each afterwards. The low speed save should be more reliable than the high speed save as the low speed save will tolerate the occasional hiccup. However, this extra reliability does not cover all possible causes of failure, e.g. problems related to frequency or bandwidth restrictions of your tape player as the period is not changed only the ratio of pulses. Finally, a file that is absolutely necessary to be retrieved from tape must be stored on two tapes. This provides a double back-up facility against accidental erasure or damage. 

"OH NO!" IT DOESN'T WORK. 

If your tape system fails to work correctly, then check the interface board or better still, have a friend check it. Eliminate any problem and re-try. If problems still exist, test the cassette player with a normal pre-taped audio tape. The music should sound normal and not flutter. If it flutters, the tape player is due for a service or replacement, or if battery operated, the batteries may be flat. Various sections may be eliminated by listening to the tape signal. If the signal saved on the tape sounds ok when played back on the player, but is not heard on the TEC, check the input section of the interface board and also the "E" output of the player with a pair of Walkman-type headphones. It is possible that the volume output is not high enough to be amplified on the interface board. This is very unlikely though on ordinary tape players but we found this to be the case with our VZ-200 data cassette player. If no signal is getting to the TEC and everything else seems to be ok, test the input buffer by setting the tape software to load and taking the input high and low with a jumper lead. The LED on the speaker should echo the inverse of the input. If not, shift the jumper to the collector lead of the input transistor and repeat the process. If the speaker LED now toggles, the input transistor is faulty. If not, investigate the latch chip. Make sure all the pins are well soldered and the feed-throughs are connecting properly. If the tape signal is heard, in the TEC speaker, but the file number is not recognised, loaded correctly, or the tape fails to load the data blocks consistently, try a better quality cassette tape. If problems persist, try a different player as the signal may be distorted or not have enough amplitude. If you still can't get it to go, a repair service is available for $9.00 plus $2.50 postage. 


## THE DAT BOARD

The Display And Tape Board • by Jim 

![](https://github.com/SteveJustin1963/tec-DAT/blob/master/schem.png)

This board will change the way you program for ever. The DAT BOARD is perhaps the most vital addition to the TEC ever. Not just a part time "add on," but rather a permanent addition to your TEC. Once you start using it, we think you'll agree. The name "DAT" is an acronym for Display And Tape. While others brawl over "their" DAT, (have you seen one?), we have quietly slipped in the back door with our version. The DAT BOARD provides these functions: 

* 16x2 LCD display. 
* Cassette tape I/O interface.
* Single stepper module.
* 5 Buffered and latched input bits.
* 1 Inverter for general use.
* Diode clipped input line. (For RS232
input)
* MON select switch.

PORT 3

Port 3 addresses an input latch. Below
is a break-down of the bits on port 3.
B IT#
0 - Serial in
1 - input 1 
