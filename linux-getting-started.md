# 1. Download toolchain
Download toolchain to build uboot and kernel at [arm-gnu-toolchain-13.2](https://developer.arm.com/-/media/Files/downloads/gnu/13.2.rel1/binrel/arm-gnu-toolchain-13.2.rel1-x86_64-arm-none-eabi.tar.xz?rev=e434b9ea4afc4ed7998329566b764309&hash=CA590209F5774EE1C96E6450E14A3E26)

# 2. Memory map overview
| No | Peripheral | Address | Size |
|:---|:----------:|:---------:|-----------:|
|01| SDRAM|0xC0000000|0x800000 (64 Mbits)|
|02| QSPI Flash|0x90000000|0x8000000 (128 Mbits)|

# 3. Connect serial console
Install stlink v2.1 driver at [offical site](https://www.st.com/en/development-tools/stsw-link009.html). Stlink v2.1 supports Virtual COM port. The USART1 is connected to the VCP. `USART1_TX: PA9`, `USART1_RX: PB7`

# 4. Build uboot


Copy to image:
- Read image from mmc
fatload mmc 0 C0000000 xipImage
- Write to QSPI Flash
sf protect unlock 0 $filesize
sf erase 0 +$filesize
sf write C0000000 0 $filesize
sf protect lock 0 $filesize

- Read image from mmc
fatload mmc 0 C0000000 stm32f746-disco.dtb
- Write to QSPI Flash
sf protect unlock 0xA00000 $filesize
sf erase 0xA00000 +$filesize
sf write C0000000 0xA00000 $filesize

bootz hel
# 5. Build kernel

# 6. Build buildroot

