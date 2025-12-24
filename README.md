# MSI Summit 13 AI+ Evo A2VM Linux Enhancements

This repository contains enhancements and fixes for running Linux smoothly on the MSI Summit 13 AI+ Evo A2VMTG laptop.

## Overview

The MSI Summit 13 AI+ Evo A2VM is a modern laptop with advanced hardware features that don't all work perfectly out-of-the-box on Linux. This repository provides:

- **IIO Sensor Support** - Enables the 6 Industrial I/O sensors (accelerometer, gyroscope, ambient light, etc.)
- **LED Control** - Fixes the audio mute/speaker LED indicators to properly reflect system audio state

## Components

### [IIO Sensors](iio/README.md)

Support for 6 IIO sensors that require vendor firmware from MSI. Includes automated firmware download and installation scripts.

**Quick Start:**
```bash
cd iio
make fetch
make install
```

See [iio/README.md](iio/README.md) for details.

### [LED Control](leds/README.md)

Enables proper hardware LED control for audio mute indicators by connecting ALSA audio controls to the kernel LED subsystem.

**Quick Start:**
```bash
cd leds
sudo make install
sudo systemctl daemon-reload
```

See [leds/README.md](leds/README.md) for details.

## Platform Information

**Model:** MSI Summit 13 AI+ Evo A2VMTG  
**Board:** MS-13P5  
**Vendor:** Micro-Star International Co., Ltd.

Verify your system matches:
```bash
cat /sys/class/dmi/id/sys_vendor
cat /sys/class/dmi/id/product_name
cat /sys/class/dmi/id/board_name
```

## Contributing

Contributions are welcome! If you've discovered additional hardware features that need Linux support or have improvements to existing components, please submit a pull request.

## License

See [LICENSE](LICENSE) for details.
