# Modix-Big-120X-Marlin-2.1.1-Firmware
Marlin 2.1.1 Firmware configured for Modix Big-120X. This is for the older version (2018-2019?) with motherboard
MKS Gen V1.4. You will see on your printer LCD screen "Marlin 1.1.9" as it boots up. This is also only configured
for English. I added multiple entries for the new menu items, but they're only added in the "language_en" file. I
apologize for that inconvenience. Feel free to reach out to me and we can try working together to get another 
language added.

Menu was significantly changed/rearranged, but the 120X calibration menu is near identical to
Modix's original menu in Marlin 1.1.9. The UBL menu has changed, but maintains the same functions with new functions added.
The change Z Height in the 120X calibration menus takes you to the Move Axis menu instead of directly
to the Z height menu. This adds another step getting there, but it's functional and conveniently allows 
you to disable soft endstops right there if needed.

The Modix manuals are still useable and followable while configuring your printer.

I have not fully tested every single menu item on the printer, but I have gone through most of it. I
believe a few items I have not used is the probe calibration and mesh validation feature in UBL.
If you find any issues I have not discovered or fixed please let me know.

I included two versions. One has the power loss recover option enabled. This uses about 87% of memory
on your printer motherboard. the other version only uses 79%. Power loss recovery has to be initiated
in Pronterface or start GCode for it to function, it is not initiated by default as it does wear out
your SD card. I have not tested the power loss recovery.

I recommend that you save your UBL Mesh before loading new firmware so you do not need to reaccomplish
the UBL if you don't need to. Do this by sending the Mesh to PC. Copy the values to Modix's mesh
editing website. Save Export the file. After loading firmware load that file in pronterface, print,
enter M500 to save. (Follow Modix's manual for better details)

Baudrate was changed to 250000. In order to connect to printer with Pronterface you will need to change
the baudrate in Pronterface from 115200 to 250000 found at the top left. Keep baudrate at 115200 while
using XLoader.

PID tuning was removed and MPC tuning is now utilized. MPC Tuning has given me more stable nozzle
temperatures compared to PID tuning, but there are a few drawbacks.
  1) You can no longer tune at different temperatures. If you need a different temperature it will
     have to be specified in the Marlin firmware, reconfigured, reuploaded to the printer, etc.
  2) I set the temperature to tune at 220 degrees. This gives pretty stable temperatures between
     200-250 degrees. The further away from 220 you set your temp to the more it fluctuates, but
     it's still only fluxes around 5 degrees when set to 200 or 250. This gives you flexibility
     in filament and temps without having to reconfigure/upload firmware.
  3) MPC tuning also accounts for the filament you're using. This has to be changed in the marlin files,
     reconfigured, reuploaded, etc. I currently have it tuning with PLA, but it also holds steady temps with
     PETG. I have not tested it with other filaments, i.e. ABS, TPU, etc, so I can't say if they will
     work or not with the current MPC tuning set-up. However, the Marlin files only has PLA or PETG
     as options to tune with.
  4) I have my values from MPC tuning entered in the firmware. If your nozzle temperature is not stable
     with my values there are 2 ways you can complete your own tune.Ensure your printer is leveled, z-offset 
     is correct, and do not have blobs of hard filament on your nozzle as it tunes with the nozzle close to the bed.
       1) Through the LCD screen. Go to Temperature -> MPC AutoTune. After completion be sure you save the
          settings. 
       2) In Pronterface. Execute "M306 T". After completion enter M500 to save the tune settings.

To change any parameters mentioned below, you will need to reconfigure and build the firmware/hex file.
I included the files I have made changes to, but you will need to download the rest from Marlin's github.
https://github.com/MarlinFirmware/Marlin/tree/2.1.x

To change filament parameters for MPC tuning:
in configuration.h - lines 702-703. Disable the PLA by adding "//" and Enable PETG by deleting the "//".

To change MPC Tuning temperature:
In .../src/module/temperature.cpp - lines 957 and 983. Replace the "220.0f" with the value of your choosing.
i.e. 200.0f, 205.0f, 250.0f, etc.

To Change back to PID tuning:
In Configuration.h - lines 646-647. Add "//" in front of "#define MPCTEMP". Delete the "//" in front
of "#define PIDTEMP". 
If you know your PIDTEMP tuning numbers, insert them into lines 661-663. Otherwise this can be done through
Pronterface after loading the new firmware.

If you would like a custom bootscreen image:
You need a small image (128x64 pixels). Best created/edited in Paint.
Go to https://marlinfw.org/tools/u8glib/converter.html, and convert the image to "Boot" screen code.
Replace the code in the .../src/lcd/dogm_Bootscreen.h file with the new code.
You can also remove the bootscreen logo altogether, or change/remove the "Modix Big-120X" machine name
in Configuration.h - lines81 and 138.

If you would like to add games to the LCD screen:
In configuration_adv.h - lines 1792-1796
Delete the "//" in front of the games you would like. (Brickout, Invaders, and/or Snake).

