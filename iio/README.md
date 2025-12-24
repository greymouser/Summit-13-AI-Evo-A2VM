# IIO Sensors

This directory contains support for 6 IIO (Industrial I/O) sensors on the MSI Summit 13 AI Evo A2VM.

## Firmware Requirement

These sensors require a firmware file from MSI to function properly.

### Download Firmware

You can obtain the required firmware in one of two ways:

1. **Manual Download**: Download version E13P5IMS.105 from MSI:
   - URL: https://download-2.msi.com/bos_exe/nb/E13P5IMS.105.zip
   - Place the downloaded ZIP file in this directory

At the time of this writing, that was the newest driver, but double check the "Chipset" tab at [this page](https://www.msi.com/Business-Productivity/Summit-13-AI-Plus-Evo-A2VMX/support?sub_product=Summit-13-AIplussign-Evo-A2VMTG#driver) for newer releases.

2. **Automatic Download**: Run the make command:
   ```bash
   make unpack
   ```

This will automatically download the firmware file to this directory and unpack it.

3. **Install**: Run the make command:
    ```bash
    sudo make install
    ```

This will copy the required firmware binary from the unpacked driver into a correct location in the system firmware directory, and renamed correctly so it cna be detected by the firmware loader.