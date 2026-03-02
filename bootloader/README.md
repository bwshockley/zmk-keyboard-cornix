### 软设备恢复说明（中文）

由于原RMK固件移除了SoftDevice，在刷入ZMK固件之前，必须先恢复SoftDevice：

1. **先用SoftDevice恢复包刷入**，地址：[bootloader/s140_restore.uf2](./s140_6.1.1_restore_no_mbr.uf2)
2. **Recovery模式进入方法**：双击RESET键进入bootloader
3. **刷入zmk固件**：使用UF2拖拽方式刷入

**注意顺序**：先SoftDevice → 后ZMK固件

    
### Extracting SoftDevice (s140) for nRF52840

Download the bootloader for nRF52840 from:

- Official repository: https://github.com/joric/nrfmicro/wiki/Bootloader
- This bootloader is compatible with nRF52840-based keyboards using ZMK firmware

These commands extract the SoftDevice (s140) from the bootloader HEX file and convert it to UF2 format for flashing:

```bash
# Convert HEX to binary
arm-none-eabi-objcopy -I ihex -O binary pca10056_bootloader-0.2.11_s140_6.1.1.hex full_flash.bin

# Extract SoftDevice section
# Skip bootloader (4096 bytes, start address 0x1000)
# Count matches SoftDevice size (151016 bytes, end address: 0x25DE7 )
dd if=full_flash.bin of=s140_sd_only.bin bs=1 skip=4096 count=151016

# method 1: generate hex file,  flashing with nrfutil or nrf connect
arm-none-eabi-objcopy -O ihex -I binary s140_sd_only.bin s140_sd_only.hex
arm-none-eabi-objcopy --change-address 0x1000 s140_sd_only.hex  s140_sd_only.hex

# method 2: generate uf2 , flash by uf2 bootloader 
# Convert bin content to UF2 format with start address for flashing 
python3 uf2conv.py s140_sd_only.bin -f 0xada52840 -b 0x1000 -c -o s140_restore.uf2
```

nRF Connect for Desktop's Programer  is very useful for getting sd address and size from bootloader hex file.

<img width="736" height="691" alt="图片" src="https://github.com/user-attachments/assets/be124646-870a-48d6-9387-d7cb043f4848" />


### Extracting SoftDevice (s130) for nRF51

Command to extract SoftDevice (s130) section:

```bash
# Skip bootloader (4096 bytes)
dd if=full_flash.bin of=s132_sd_only.bin bs=1 skip=4096 count=155648
```
