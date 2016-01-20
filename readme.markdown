This is a log of my efforts trying to bring a computer that seems to date from around 1993 back to life.


19/01/2016
=====

The system has a diskette reader and no CD reader. It has several ports on its back in a total of 9 ports. One of them is an Ethernet port. A VGA port seems to be the image output.

On the inside three RAM sticks exist. The processor is an Intel DX20DPR66 (66.00 MHz, 8 kB L1 cache, FPU). The hard drive has 425.3 MB.

The cabling on the inside is messy. The power source has lots of black dust on its ventilation openings. The BIOS battery seems to be an AA battery, which is inside a plastic box. A loudspeaker exists. This might be what is used as a buzzer by the motherboard.

I previously tried to turn it on but there was no image. Only some beeps. I don’t know which computer this is. I plan to take it apart, vacuum it, find out which motherboard this is and from there figure what might be going on.

20/01/2016
=====

#### 3pm

Powered the computer and turned it on. The power source ventilates, the disk starts to move, the buzzer beeps some sequence (which I recorded), the diskette reader seems to make some noise too for a moment (or is this a sound from the disk?). I connected the bottommost connector to a VGA cable an onto a monitor. No image was produced. Since there is a pattern of beeps when the machine is turned on maybe the BIOS is reporting some kind of error.

I thought of connecting a PS/2 keyboard to the computer and trying to power it again. It is unlikely, but maybe the computed refused to start because no input devices (mouse or keyboard) was present when it was turned on. I grabbed a PS/2 keyboard and was going to happily connect it... But then I realised there are no PS/2 inputs!! What am I going to do with this thing?!

#### 11pm

Didn't take the computer apart yet. Found a motherboard that looks very similar to the one in the computer. It seems to be a Gemlight GMB-486SG v2.2. Maybe the one in the computer is another version of this motherboard.

From the [GMB-486sg v2 manual](doc/GMB-486sg-v2_603.pdf) I learned that the keyboard input is a "standard five-pin female DIN" connector. PS/2 is essentially a mini-DIN connector used in IBM Personal System/2 series of computers. DIN to PS/2 convertion seems to be straighforward: I only need to connect the respective wires. PS/2 has seven pins, but two of them are not connected. This is good. I should now be able to connect a keyboard to the computer!

![Gemlight GMB-486SG v2.2](img/Gemlight_GMB-486SG_v2.2.jpg)

