Monkey Island 2, LeChuck's Revenge: FlashFloppy AutoSwap v1.6
=============================================================
-- Keir Fraser, 12th March 2019

Introduction
------------

This version of the game is patched to swap disks automatically when
used with a Gotek programmed with FlashFloppy >= v1.0.

Getting Started
---------------

The simplest method is to copy the whole distribution folder as-is to
your USB drive.

If you directly select images using LCD/OLED screen you can now
navigate to disk 1 and all further disk changes will be automatic.

If you use the Autoboot file selector you must assign all disks,
including the save-game disk, to slots -- the exact slot numbers do
not matter. You can now start the game by navigating to the slot for
disk 1.

Normal game requests for a disk change are mostly suppressed. Any
which do appear ("Insert xx" box in the middle of the screen) can be
ignored, simply left click to continue and the change will happen
automatically.

Renaming Disk Images
--------------------

If you do not like the default names for the disk images they may be
changed, with some restrictions. All the images must have the same
name pattern (eg. "Monkey Island 2 (%).adf") where % is the 2-digit
disk number or "Save" for the save-game disk:
"Monkey Island 2 (01).adf", "Monkey Island 2 (Save).adf", and so on.

Load / Save
-----------

A blank save disk is included in this distribution, and will be
mounted automatically when you select "Load" or "Save" within the
Load/Save requester. The message at the top of the requester to
manually insert the save disk should be ignored.

If you wish to use your own save-game disk, you must replace the
provided one, using the same image name, and the AmigaDOS disk label
*must* be "Monkey2 Save". You can use the RELABEL command in the Amiga
Shell/CLI to set the disk label.

Other Issues
------------

Mouse pointer may be vertically misaligned (too high on the screen)
with Kickstart v3 and later. This is fixed by downgrading Kickstart
(eg. use a kickstart switcher; or Relokick, if there's sufficient RAM).

Another workaround is to boot from the supplied alternative Disk-1
image (unzip "monkey2_alt_boot.zip" over the old boot disk). This
version does not vertically centre the display on PAL systems, which
seems to circumvent this issue.

Alternatively, press LMB or Fire during boot to switch to NTSC mode
(may not work reliably with all Kickstart versions). This works best
witnh the alternative Disk-1 image, as it is vertically centred on
NTSC screens.

Feedback / Bug Reports
----------------------

Send to the Facebook group (https://www.facebook.com/flashfloppy) or
raise an issue on Github (https://github.com/keirf/FlashFloppy).
