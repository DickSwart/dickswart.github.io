Using the [Wake on LAN Switch](https://www.home-assistant.io/components/switch.wake_on_lan/) to turn my remote windows 10 PC off and then waking it up again.

Wake on LAN (WOL) will only work if the computer _supports_ it and it is _enabled_.

## Configuration on the computer you want to switch on/off

### Enable Wake on LAN in BIOS
1. To enable Wake on LAN you will need to restart your computer and enter the BIOS _(Normally by pressing `F`, `F12` or the `Del` key on your keyboard)_.
2. Go to power management and look for Wake on Lan, make sure it is enabled.
3. Save and exit the BIOS (Normally the `F10` key).

### Software
We will need to install software to enable us to put the PC in sleep mode as there is no _"out of the box"_ functionality to do this. It is possible to shut down the PC without any software but if you do that you cannot wake the PC.

Wake on LAN will only work if the PC is in Hibernate or Sleep mode.

### References
* Home Assistant Wake on LAN Switch - https://www.home-assistant.io/components/switch.wake_on_lan
* sleep-on-lan - https://github.com/SR-G/sleep-on-lan
* Wake-on-LAN wikipedia - https://en.wikipedia.org/wiki/Wake-on-LAN


