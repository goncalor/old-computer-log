This is a log of my efforts trying to bring a computer that seems to date from 1994 back to life.


19/01/2016
=====

The system has a diskette reader and no CD reader. It has several ports on its back in a total of 9 ports. One of them is an Ethernet port. A VGA port seems to be the image output.

On the inside four RAM sticks exist. The processor is an Intel DX20DPR66 (66.00 MHz, 8 kB L1 cache, FPU). The hard drive has 425.3 MB.

The cabling on the inside is messy. The power source has lots of black dust on its ventilation openings. The BIOS battery seems to be an AA battery, which is inside a plastic box. A loudspeaker exists. This might be what is used as a buzzer by the motherboard.

I previously tried to turn it on but there was no image. Only some beeps. I don't know which computer this is. I plan to take it apart, vacuum it, find out which motherboard this is and from there figure what might be going on.


20/01/2016
=====

#### 15h

Powered the computer and turned it on. The power source ventilates, the disk starts to move, the buzzer beeps some sequence (which I recorded), the diskette reader seems to make some noise too for a moment (or is this a sound from the disk?). I connected the bottommost connector to a VGA cable an onto a monitor. No image was produced. Since there is a pattern of beeps when the machine is turned on maybe the BIOS is reporting some kind of error.

I thought of connecting a PS/2 keyboard to the computer and trying to power it again. It is unlikely, but maybe the computed refused to start because no input devices (mouse or keyboard) was present when it was turned on. I grabbed a PS/2 keyboard and was going to happily connect it... But then I realised there are no PS/2 inputs!! What am I going to do with this thing?! I didn't realise this earlier because the computer has what looks like a female mini-DIN-8 connector, which at a first glance I thought was a PS/2.

#### 23h

Didn't take the computer apart yet. Found a motherboard that looks very similar to the one in the computer. It seems to be a Gemlight GMB-486SG v2.2. Maybe the one in the computer is another version of this motherboard.

From the [GMB-486sg v2 manual](doc/GMB-486sg-v2_603.pdf) I learned that the keyboard input is a "standard five-pin female DIN" connector. PS/2 is essentially a mini-DIN connector used in IBM Personal System/2 series of computers. DIN to PS/2 conversion seems to be straightforward: I only need to connect the respective wires. PS/2 has seven pins, but two of them are not connected. This is good. I should now be able to connect a keyboard to the computer!

![Gemlight GMB-486SG v2.2](img/Gemlight_GMB-486SG_v2.2.jpg)


21/01/2016
==========

#### 11h30

From the motherboard's manual

    Currently there is only one beep code in BIOS. This code indicates
    that a video error has occurred and the BIOS cannot initialize the
    video screen to display any additional information. This beep code
    consists of a single long beep followed by two short beeps.

What I hear when turning it on is actually a long beep followed by two short ones followed by another short one. Something like `---- - -  -`. The manual mentions only two beeps. But the motherboard is not exactly the same as the one in the manual so that's fine (still need to figure which version the computer has).

So the computer is complaining it cannot initialise the screen. The manual also mentions errors that may be caused because the CMOS battery is weak

    CMOS BATTERY HAS FAILED:
    CMOS battery is no longer functional. It should be replaced.

    CMOS CHECKSUM ERROR:
    Checksum of CMOS is incorrect. This can indicate that CMOS has
    become corrupt. A weak battery may have caused this error. Check
    the battery and replace if necessary.

The battery is most probably dead in a computer with 20+ years, that much I figured. But now I'm sure this may be preventing the system to boot. Replacing the battery might be all that is needed for it to boot.

#### 12h30

The battery plastic box is fastened to the chassis with velcro. The box seems to be a bit damp: the battery may be leaking.

Nope, it's not leaking. The battery turns out to occupy only half the space in the box. It's a 1/2 AA battery! It's a TL-5151, 3.6 V and unknown capacity. I should check if it's dead, but I don't have a multimeter (should buy one). I guess I will use one LED and a resistor to see if it lights up.

A green LED (2.1 V?), a 220 and a 330 resistors in parallel. That should be about 9.1 mA through the LED. Nothing happened, as expected. This battery is dead (going to save it to test later with a multimeter, I guess). I think I will make a series of two AA batteries and test this. I don't have any 3.6 V battery.

BTW: there seems to be no on-board battery. The place for the battery is there but there is none soldered. Unless it's on the underside of the motherboard. An external battery can be used connected to JP1 (marked on the board, it says J1 on this manual) and that is where the TL-5151 is connected.

#### 15h

Grabbed a 2 AAA battery support I had laying around. Substituted the 3.6 battery by this 2 AAA battery combo (3V total). Powered the computer. Still the same beeps. Either the problem is not (only) from the battery or 3V won't do it.

#### 18h

Since I still don't know if the problem is the lack of battery I tried to check this again. I don't have any 3.6 V battery, but I have an Arduino which can supply 3.3 V. I powered JP1 with 3.3 V from the Arduino. Result: same ol' beep. I think I can pretty much rule out that the problem is from the lack of battery. I guess there is nothing else to do without disassembling the thing first.

#### 22h40

Before disassembling there is one more (stupid) thing I had to try: turn it on with no battery at all. I just did. Nothing new happened.


22/01/2016
==========

#### 9h30

This motherboard manual has no date. There is one BIOS example that has date 1994 though. None of the referred CPUs have a frequency greater than 66 MHz. Is it possible that the motherboard on the computer is a version greater than v2.2?

I read about some systems that won't POST (power-on self-test) if the RTC (real-time clock) battery is dead. This is another hypothesis.

#### 11h00

Started to disassemble and vacuum. The motherboard reads

    MD-4DUV-VER.: 2.0
    MADE IN TAIWAN R.O.C.

I'm looking up this motherboard, trying to find a manual. Found several lookalikes on eBay, but none equal. Now I know this is not a Gemlight GMB-486SG v2.

I can't find MD-4DUV, but I can find M*B*-4DUV/UVC. Maybe the D is a typo in the motherboard? I can't find a PDF with all the info regarding any of these either. Nonetheless I found a web page with all the [jumper configurations for MB-4DUVC model 2][MB-4DUVC-m2]. This web page has a schematic for the board, identifying the positions of the main components on the board. The jumper positions on the computer match the ones of the schematic. But the schematic mentions a "KBD" to the right, close to JP1. JP3 seems not to exist on the board (close to KBD). Also the TAG seems not to exist, or at least it is not in the same position as in the schematic.

![MB-4DUVC model 2](doc/MB-4DUVC_MODEL_2.png)

[MB-4DUVC-m2]: doc/MB-4DUVC_MODEL_2.html

#### 17h15

Something interesting is happened: I'm in the process of disassembling the computer. I removed all boards except the video board. Then I turned on the computer (so no disk drive, no diskette drive) without anything connected to the output of the video board. This time there was a single beep!! I tried to do it again, and only one beep again. Then I tried to connect a VGA monitor: the old four beeps error returned. I disconnected the monitor, still four beeps. I haven't yet figured out what is defining if the error is triggered or not. Working on that. But hey! This is the first time there is a single beep!

Now with the same setup I'm getting sometimes a lower tone, longer 4 beeps. Sometimes followed by a single higher pitch (the original one) single beep. I can't figure what makes the changes, but what I have in mind is whether there is a cable connected to the VGA port; and whether the port from the board is touching the metal surrounding it or not. This second consideration happens only because the bolts of the VGA port are missing, meaning the port is not fixed to the I/O shield. This might result in some grounding problem.?

Ah, BTW, I don't even have the CMOS battery in place. I removed the JP1 jumper.

Now I put a bolt on the VGA, so now it is connected to the I/O shield. When I turn the power on or reset (with the reset button) there are four low pitch, long beeps followed by a short high pitch one. This now happens consistently.

Connected the 2 AAA batteries to JP1. The same beep pattern continues. Time to remove the video card.

#### 18h30

With the graphics card removed the original four beeps occur (the ones described on 21/01/2016).

Now removing the RAM. There are four slots, each with a RAM stick with 8 HM514400BS6. This IC has 1 Mwords each with 4 bits. Since the system is 32 bits the 8 chips make a complete 32 bit word. Therefore each RAM stick has 4 MB, or 1 MDWORD. 16 MB total on-board. Yay!

With no RAM present the computer makes a long beep stops for a bit and does it again. It seems to do this infinitely. With only one stick (in BANK 0) the 4 old beeps return. Also tried only BANK 3: same result.

#### 20h

All RAM back in place. The video card too (using SL3, instead of the original SL1). From what's written on the board it seems connecting pins 3 and 4 of JP1 resets the CMOS. I tried this. With the jumper there there are no beeps at all when the computer is turned on. Is this good? The monitor still recognises no image though.

#### 20h15

Put the 2 AAA batteries on JP1. Video and disk cards are in place (no disks connected though). Got image for the first time! The image is not good though. But I think I can distinguish the energy star logo on the upper right corner! This may be starting to get somewhere. I think when it booted the four low pitch beeps plus the high pitch one occurred (not sure).

![Distorted AwardBIOS. Energy Star logo can be discerned](img/Energy-Star_distorted.jpg)

OMG! I removed the VGA cable, rebooted it, inserted the VGA cable and there! The AwardBIOS image was there, the memory check was being performed, the Energy Star logo nicely done this time... Success! I got image!

The image was a bit to the left of the monitor and was cut. I adjusted that on the monitor. The motherboard that is stated by the AwardBIOS is MB-4DUV VER 3.0. The processor is 80486DX-S at 66 MHz. The memory is 16384K, exactly 16 MB, as expected.

![AwardBIOS main screen](img/AwardBIOS_main.jpg)

Now I need a keyboard to interact with this thing :) I will try to adapt PS/2 to 5-pin DIN, since this should be straightforward, just requiring some wiring.

Well, the bad news are that this is inconsistent: I power the computer off and tried to boot it again to see if I got the same results. I did not. No image again. Must try to figure how to reproduce the previous success.


23/01/2016
==========

It seems the AwardBIOS POST beep codes are the ones in the following table. From these a problem with the video card seems to exist, which only makes sense.

Beeps                    | Meaning
------------------------ | -------------------
1 short                  | "All systems clear"
1 long, 2 short          | Video adapter error: Bad or improperly seated video card
Repeating beeps          | Memory error: Bad or improperly seated RAM
1 long, 3 short          | Bad video RAM or video card not present
High-frequency beeps     | Overheated CPU: Check fans
Repeating high/low beeps | CPU: Improperly seated or defective CPU

I removed the video card and am examining it. There are several ICs:

IC               | Description           | Manufacturer
---------------- | --------------------- | ----------------
MB81C4256A-70P   | CMOS dynamic RAM      | Fujitsu
CL-GD5428-80QC-A | VGA controller        | Cirrus Logic
CL-GD5...?       | BIOS ROM ?            | Cirrus Logic
GAL116V8B        | E<sup>2</sup>CMOS PLD | Lattice Semiconductor
SN74F32          | OR gates array        | Texas Instruments
SN74F245N        | octal bus transceiver | Texas Instruments

Now, the Lattice GAL116V8B programmable logic might be a problem: according to it's datasheet there is 20 year data retention. Since the computer appears to date from around 1994 and we are in 2016... The EE memory might not have the original data anymore. Nonetheless I'm unsure if this is the case, since I was able to get image from this card once already.

One of the Cirrus Logic ICs has a sticker on its top, so I can't know which IC it is. But I think this might be the video card BIOS chip. I can't find information on the graphics card online. The board reads

    VGA-5426
    PCB REV. C
    FCC ID: GPLAPC-5426
    MADE IN TAIWAN

Still on the GAL116V8B problem theory: maybe the memory is not fully retained anymore an it is in a state that sometimes it outputs the programmed values and sometimes it doesn't? I don't know what happens when a EE memory expires its data retention time, but this is a possibility. With this in mind the one and half times the computer worked maybe I was lucky enough for the GAL116V8B to output what it should.

#### 15h30

Oh my... Was the problem a dirty graphics board pin? There was a little dot of something on one of the VESA Local Bus pins. I scratched it with the tip of a mechanical pencil and also scratched all others with my nails. I inserted the graphics card, connected the VGA cable, powered the computer... A single short beep! And voilà: the AwardBIOS main screen shows up on the monitor. I rebooted two times and I worked every time!

The AwardBIOS performs the memory test and additionally it complains about no keyboard being present and that there was a CMOS Checksum error. Also I confirmed that the CMOS battery on JP1 is not needed for the system to boot.

    Keyboard error or no keyboard present
    CMOS checksum error - Defaults loaded

If I leave the 2 AAA batteries on JP1 and reset the computer the CMOS error still appears. So I guess the settings are not being saved. Maybe 3 V is not enough voltage to keep the settings. Or there is another problem that prevents the settings from being preserved.

Now I need to make a PS/2 to 5-pin DIN adapter. Since the GAL116V8B might die (20 year data retention) I should also consider mapping it so that I can make a copy of it if needed.

Tried to reset the CMOS battery by using the 2-3 jumper on JP1. For this I shut down the system, put the jumper there, powered, waited a few seconds, shutdown, removed the jumper, powered. Not sure if this is the correct procedure to reset the CMOS (I don't have a manual!). The fact is that the CMOS checksum error is still there.


25/01/2016
==========

Read the datasheet for GAL116V8B. It's not as easy to map as I thought. To be honest I don't understand it too well from the datasheet. It has several modes and the datasheet does not specify how it can be programmed. All I know is there is some compiler. There is a "security cell" feature which "prevents unauthorized copying of array patterns". If this was activated when manufacturing I cannot hope to make a direct copy of the configuration. This mode can only be disabled by re-programming the device, which prevents the examination of the original configuration.

On the keyboard adaptor side: I'm thinking of making a connector, but I think it's difficult to make female connectors. I need a female PS/2 and a male 5-pin DIN. I could buy both instead, but I don't feel like it...


17/02/2016
=========

Well, I'll just buy a DIN 5 male PS/2 female converter. But before I decide on that I should also see if I would be able to connect a mouse. Another issue is how to put an OS into this hard drive... It's an IDE hard drive and the computer has no USB (the 1.0 specification was introduced only in January 1996, so...) and no optical disk drive. All I have is a floppy drive. I feel pretty much locked out about this. Nonetheless I believe the disk must still have some OS in it. I have no idea if it has a password.

Hm... What if this computer didn't even have a mouse?! I'm looking at the board I suspect must be the one where the mouse would connect, but I can't find info on it. Actually it has a FCC ID on it: FSUGSG5A. On the FCC website it says

    Application: Hand-Held Scanner
    Equipment Class: JBP - Part 15 Class B Computing Device Peripheral

So it seems this is for a scanner, not for a mouse. Or can a mouse be in a way a "hand-held scanner?" BTW, the connector that the board has to the outside of the computer is a 8 pin mini-DIN connector.

The Ethernet card has an RJ45 connector and an AUI (Attachment Unit Interface) female connector.

I can't think of any place where the mouse could be connected. Maybe 1) there was no mouse at all (seems strange...? But I don't know which OS this had) or 2) the card where the mouse connected was removed.

18/02/2016
=========

I just checked that mouses exist with 8 pin mini-DIN interface. Therefore I think that board really is where a mouse connects.

It seems that 8 pin mini-DIN was used at least by Sun Microsystems to aggregate both keyboard and mouse in a single connector. There is a mouse in pin and both a keyboard in and out pins. This probably means I could wire a PS/2 mouse into this 8 pin connector and make it work.

23/02/2016
==========

A few weeks ago I found a computer on the street. It's from around 2006 (it has a Windows Vista license sticker on it, and Vista was released to manufacturers starting on 8 November 2006). This computer happens to have a DVD IDE drive. This means I now have a drive to put on the computer this log is about. This upgrades my possibilities of putting software into the hard disk. I'm not sure if a DVD drive will work there, but I suppose since the interface is IDE it should work.

12/04/2017
==========

Long time no see. I ordered a 5-pin DIN male to PS/2 female adapter. It arrived today. I already checked that it fits the connector for the keyboard but I don't have time to fire up the computer right now.

18/04/2017
==========

I have time to work on the computer again. Finally.

I used the keyboard adapter which arrived the other day to connect the keyboard. I worked nicely with a PS/2 keyboard (it is possible that it doesn't work with some of the current USB keyboards, but I have tried none).

To start with I explored the BIOS. There's a bunch of interesting stuff there. One which struck me is that there's an option to select between "write-through" vs "write-back" caching. It is also looks like it is possible to select two memory regions which are not cached. There are power settings too. For example after how many minutes the disk should be stopped if inactive.

After I check the BIOS I was ready to try and boot the OS. But... I had disconnected the disk, diskette, etc. Newer IDE cables can only be connected one way, because the interface has a missing pin and the cable has plastic on the corresponding place. However, these old cables have no such plastic granting that the cable is only inserted one way. And as far as I can tell there's no way to figure this out from the interfaces it's connecting to.

An aside: it seems the term "IDE" is old and this type of interface should now be referred to as PATA (Parallel ATA), the predecessor of Serial ATA (SATA). However, IDE was the name originally used by Western Digital and IDE drives were first used in Compaq PCs in 1986. The disk on this computer is a Western Digital's Caviar 2420, so the term IDE was probably still how it was called at the time this computer was built (even today people refer to IDE...). I'll use "PATA" from now on here.

So... I had to check the photos I took when I was first opening and cleaning the computer to try and figure out how the was connected. Some of the photos had what I needed. So I connected the PATA data cable and the LP4 power cable, I powered the computer aaaaaand... the disk did not move at all. I turned off the computer and tried again with the same results. Then I noticed because the original connections were messy one of the photos misled me and I connected the cable end at the side of the motherboard incorrectly. Damn... I hope I did not damage anything... I reconnected the cable in the correct way but the result was still the same (except now I was in doubt whether I had screwed anything up, which sucks). The disk is not detected by the computer either, which says

    DISK BOOT FAILURE, INSERT SYSTEM DISK AND PRESS ENTER

I decided to put the disk into another computer with PATA to see if the disk was okay. There everything went smoothly! The computer booted into Windows 3.1! There's Microsoft Word 6.0, PowerPoint 3.0, Painbrush 3.11 (that's the previous name of Paint), McAfee VirusScan 3.1.0, MATLAB 4.2c, Netscape Navigator 3.04, ... Many interesting stuff! Oh, and the screensaver is pretty nice too. I don't have a mouse for this computer so I had to check all of this with the keyboard only.

Well, so I know the disk is working. Before I mess anything up I decided to backup the entire OS. I booted Arch GNU/Linux (it's what I currently have on that machine) and made a raw copy of the 425 MB disk with `dd`. This took like 3 minutes (I didn't really count). To check if my copy was OK I converted it into a VMDK file to try and boot the OS on VirtualBox.

    qemu-img convert -f raw win31.iso -O vmdk win31.vmdk

I made a video showing the OS booting and opening some programs and screensavers. You can [watch it here][win31-boot]. Remember this is running on a host with a much more powerful processor and disk, so everything is pretty fast compared to running on the actual hardware. I could have slowed down the processor for example but that would just result in you waiting more time to see it boot.

I put the disk back in the old computer and it still didn't rotate. So I removed the board that controls the hard drive and diskette to see if the pins were clean (after all the problem with the video card was a dirty pin). They were not clean. I just scratched them a little with my nails, but they would benefit from a more through cleaning. I inserted the board back and now the disk rotates. But! it is not detected by the BIOS. So, now I think the problem might be one from these: some dirty pins are not making contact; it is necessary to configure something on the BIOS (although there's a feature to autoconfigure disks, that right now can't find any); or I damaged something when I connected the PATA cable incorrectly. My first bet will be on the dirty pins.

[win31-boot]: https://www.youtube.com/watch?v=-mOKU5esBJU

20/04/2017
==========

I just finished cleaning the pins of the board to a point where if the disk isn't detected now I don't think its because of dirty pins.

While cleaning the board I saw that the PATA interface on the board is numbered. The ribbon cable has a red wire which is wire number 1, which connects to the number 1 pin on each side. If I knew/had noticed that before I wouldn't have connect the cable in the wrong way.

I just powered the system. First, I received a `DISK BOOT FAILURE`. So I tried the BIOS `HDD IDE AUTO DETECTION`. This time the disk was instantly recognised and its details were filled for Drive C. Great!

This looked promising so I tried to boot into the OS. Only to get a `Missing operating system`... I tried to repeat the disk detection again and this time no disk was detected. Then I powered off the computer and tried again. The disk was detected but with wrong information (160 MB filled vs 405 MB real, for example). Hm... what could be the problem?

I managed to get the `Missing operating system` error again. Why is the operating system "missing"?... I put the disk back in the other computer and there is no doubt that the disk is working and the OS is there...

#### 20h20

It's working!! It booted! Once again the damn problem were dirty pins! I took the board out and looked at the pins again. This time I used fine sandpaper to make them really shiny. Then I reinserted the board, made the BIOS detect it (remember I have no CMOS battery, so the settings don't persist), restarted and voilà!

Now, the next nice step would be to make Ethernet work. Getting a CMOS battery would also be nice. I don't have a mouse for this either, which difficults things a little. Some things I don't know how to to with the keyboard only (or they can't be done at all).

21/04/2017
==========

I inserted the network interface controller/card (NIC). The computer boots and now the driver doesn't complain. There's no error right after starting up either (as there was in the video above). Also, running `ipconfig` does not error out as it did before. It actually shows a (static) IP. I tested this with no cable connected to the card, yet.


License
=======

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International license][license].

![BY-SA badge][by-sa-badge]

[license]: http://creativecommons.org/licenses/by-sa/4.0/
[by-sa-badge]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
