FIRMWARE = 13P5_ISH_WHQL_PR2_5.8.30.0_0x44cef6da
FIRMWARE_FILE = $(FIRMWARE).zip
FIRMWARE_URL = https://download.msi.com/nb_drivers/cs/$(FIRMWARE_FILE)
HASH_FILE = .$(FIRMWARE_FILE).sha256
UNPACK_DIR = firmware
DESTDIR ?=
FW_DIR = /lib/firmware/intel/ish

# DMI paths for ISH firmware naming
DMI_SYS_VENDOR = /sys/class/dmi/id/sys_vendor
DMI_PRODUCT_NAME = /sys/class/dmi/id/product_name
DMI_PRODUCT_SKU = /sys/class/dmi/id/product_sku

# Calculate CRC32 of DMI field using Python
define crc32
$(shell python3 -c "import zlib; print(format(zlib.crc32(open('$(1)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))" 2>/dev/null || echo "")
endef

.PHONY: fetch clean unpack install show-firmware-name

fetch:
	@if [ -f $(FIRMWARE_FILE) ] && [ -f $(HASH_FILE) ]; then \
		CURRENT_HASH=$$(sha256sum $(FIRMWARE_FILE) | cut -d' ' -f1); \
		SAVED_HASH=$$(cat $(HASH_FILE)); \
		if [ "$$CURRENT_HASH" = "$$SAVED_HASH" ]; then \
			echo "Firmware file already downloaded and verified."; \
			exit 0; \
		fi; \
	fi; \
	echo "Downloading firmware from MSI..."; \
	wget -O $(FIRMWARE_FILE) $(FIRMWARE_URL); \
	sha256sum $(FIRMWARE_FILE) | cut -d' ' -f1 > $(HASH_FILE); \
	echo "Firmware downloaded successfully: $(FIRMWARE_FILE)"

unpack: fetch
	@echo "Unpacking firmware..."
	@mkdir -p $(UNPACK_DIR)
	unzip -o $(FIRMWARE_FILE) -d $(UNPACK_DIR)
	@echo "Firmware unpacked to $(UNPACK_DIR)/"

install: unpack
	@echo "Installing ISH firmware..."
	mkdir -p $(DESTDIR)$(FW_DIR)
	@ISH_FILE=$$(find $(UNPACK_DIR)/ISH_Kit_*/ISH_SW/OS*/Drivers/Win*/*ExtensionTemplate/x64/FwImage/00*/ishS_SI.bin 2>/dev/null | head -n1); \
	if [ -z "$$ISH_FILE" ]; then \
		echo "Error: Could not find ishS_SI.bin in unpacked firmware"; \
		exit 1; \
	fi; \
	VENDOR_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_SYS_VENDOR)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	PRODUCT_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_PRODUCT_NAME)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	SKU_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_PRODUCT_SKU)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	FW_NAME="ish_lnlm_$${VENDOR_CRC}_$${PRODUCT_CRC}_$${SKU_CRC}.bin"; \
	echo "Found ISH firmware: $$ISH_FILE"; \
	echo "Installing as: $$FW_NAME"; \
	echo "  SYS_VENDOR:   $$(cat $(DMI_SYS_VENDOR)) (CRC32: $$VENDOR_CRC)"; \
	echo "  PRODUCT_NAME: $$(cat $(DMI_PRODUCT_NAME)) (CRC32: $$PRODUCT_CRC)"; \
	echo "  PRODUCT_SKU:  $$(cat $(DMI_PRODUCT_SKU)) (CRC32: $$SKU_CRC)"; \
	install -m 644 "$$ISH_FILE" "$(DESTDIR)$(FW_DIR)/$$FW_NAME"

show-firmware-name:
	@VENDOR_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_SYS_VENDOR)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	PRODUCT_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_PRODUCT_NAME)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	SKU_CRC=$$(python3 -c "import zlib; print(format(zlib.crc32(open('$(DMI_PRODUCT_SKU)', 'rb').read().rstrip(b'\n')) & 0xffffffff, '08x'))"); \
	echo "Firmware filename: ish_lnlm_$${VENDOR_CRC}_$${PRODUCT_CRC}_$${SKU_CRC}.bin"; \
	echo "  SYS_VENDOR:   $$(cat $(DMI_SYS_VENDOR)) (CRC32: $$VENDOR_CRC)"; \
	echo "  PRODUCT_NAME: $$(cat $(DMI_PRODUCT_NAME)) (CRC32: $$PRODUCT_CRC)"; \
	echo "  PRODUCT_SKU:  $$(cat $(DMI_PRODUCT_SKU)) (CRC32: $$SKU_CRC)"

clean:
	rm -f $(FIRMWARE_FILE) $(HASH_FILE)
	rm -rf $(UNPACK_DIR)
