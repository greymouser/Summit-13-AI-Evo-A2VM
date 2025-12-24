# LED Control for MSI Summit 13 AI+ Evo A2VMTG

This directory contains components to enable proper LED control on the MSI Summit 13 AI+ Evo A2VMTG laptop, specifically for the audio mute/speaker LED indicators.

## What This Does

The MSI Summit 13 AI+ Evo A2VMTG has hardware LEDs that should indicate the audio mute state, but the speaker mute doesn't work out of the box on Linux. This package bridges the gap by:

1. **Detecting the platform** - Verifies you're running on the correct MSI laptop model using DMI information
2. **Monitoring audio state** - Watches the ALSA audio control for mute/unmute events
3. **Controlling the LED** - Connects the audio control to the kernel's LED subsystem so the hardware LED responds correctly to audio state changes.

## Components

- **msi-ctl-led-attach** - Script that attaches the ALSA audio control to the kernel LED subsystem
- **msi-ctl-led-attach.service** - systemd service that runs the attachment script
- **80-msi-ctl-led.rules** - udev rule that triggers the service when sound devices are detected
- **msi-ctl-led.conf** - Configuration file with platform identification and ALSA control settings

## Installation

```bash
sudo make install
sudo systemctl daemon-reload
sudo udevadm control --reload-rules
```

After installation and a reboot (or replugging audio devices), your mute LED should properly reflect the system audio mute state.

## Configuration

The default configuration in `msi-ctl-led.conf` should work for most MSI Summit 13 AI+ Evo A2VMTG systems. If you have a different variant, you may need to adjust:

- `WANT_VENDOR`, `WANT_PRODUCT`, `WANT_BOARD` - Platform identification from DMI
- `CARD_NAME` - ALSA card name (usually `card0`)
- `CONTROL_NAME` - ALSA control to monitor (usually `Master Playback Switch`)

Check your system's DMI values:
```bash
cat /sys/class/dmi/id/sys_vendor
cat /sys/class/dmi/id/product_name
cat /sys/class/dmi/id/board_name
```
