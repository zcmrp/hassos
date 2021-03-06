# Odroid-XU4

## eMMC

The Odroid XU4 has a hidden boot sector that is only visible on the Odroid itself (can't be written by a card reader). There are a couple possibilities:
1) If the eMMC already had a working image before flashing HassOS:
* It will be booting to uBoot (but no further). 
    * If you have the serial adapter, you should be able to enter `distro_bootcmd` at the uboot prompt to continue booting.
    * If not, flash the HassOS image to an SD card and boot off that temporarily (while the eMMC is also plugged in).
* Once booted, login at the prompts and then enter `dd if=/dev/mmcblk0 of=/dev/mmcblk0boot0 bs=512 skip=63 seek=62 count=1440` at the linux prompt.
* Reboot with eMMC (don't forget to flip the boot switch to eMMC)
2) Clean/wiped/corruped boot sector:
* You'll need to follow [Hardkernel's instructions](https://forum.odroid.com/viewtopic.php?f=53&t=6173) to get a working boot sector. Then flash HassOS and follow instructions above.
* Alternatively, you can try flash HassOS to both an SD and eMMC, then boot off the SD with the eMMC also plugged in, then run `dd if=/dev/mmcblk1 of=/dev/mmcblk0boot0 bs=512 skip=1 seek=0 count=16381` at the Linux prompt. Note that this is untested, but in theory should work..

## Console

By default, console access is granted over the serial header and over HDMI. Certain startup messages will only appear on the serial console by default. To show the messages on the HDMI console instead, swap the order of the two consoles in the `cmdline.txt` file on the boot partition. You can also delete the SAC2 console if you don't plan on using the serial adapter.
eg. `console=tty1 console=ttySAC2,115200`

## GPIO

Refer to [the odroid wiki](https://wiki.odroid.com/odroid-xu4/hardware/expansion_connectors).
