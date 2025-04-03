# Haj's Adventures (CSIT)

A little blahaj-themed game with 3 flags hidden inside. 

## Flag 1
Hidden inside `jellyfish.py`. Copy and paste the `generate_flag()` function to profit!

Patching the code for the last jellyfish (to the right of the level, that can’t be collected) also works and gives you the flag. You need quick fingers to screenshot, though, because the displayed flag text flashes.

There’s actually a kiwi file present only as bytecode in `__pycache__`, but its `generate_flag()` function is the same as jellyfish.

## Flag 2

Hidden inside `SharkCaveLauncher.exe` (a .NET binary).

I attempted (unsuccessfully) to reverse the image generation :(

A bunch of text is taken from the ends of the files with numbers on the end and assembled into the string “centreforstrategicinfocommtechnologiessg”. This is used as the XOR key for the data in ` divereallydeep.txt`, which is a bunch of B64 for an image. Unfortunately, I couldn’t get the image size right :dies:

The contents of the "bigandsafe" and "machinewashable" files are found to be the possible X and Y coordinates for a "treasure". When you click on that location, the flag will be shown at the top-left corner of the window. The treasure coords are randomly selected from the lists in the aforementioned files each click.

I just took two locations (10, 98) and kept clicking until I got the flag.

![](images/flag2.png)

*Just keep clicking until the RNG works. Definitely not because I couldn’t get WinDbg to work with the .NET binary.*

## Flag 3

Hidden in the spectrogram of `Assets/Music/congrats.wav` spectrogram (as usual).
