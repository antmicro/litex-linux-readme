# Running Linux on LiteX based Risc-V SoC

# Getting the toolchain

wget www.antmicro.com/downloads/riscv-gnu-toolchain.tar.gz

# Getting the sources

`git clone https://github.com/antmicro/litex-linux-riscv.git`

# Building

kernel configuration:

```
cd litex-linux-riscv
cp litex_default_configuration .config
ARCH=riscv CROSS_COMPILE=CROSS_COMPILE=riscv32-unknown-linux-gnu- make -j`nproc`
riscv32-unknown-linux-gnu-objcopy -O binary vmlinux vmlinux.bin
```

devicetree:

```
dtc -I dts -O dtb -o vmlinux.dtb rv32.dts
```

# Litex SoC

Litex requires different gcc toolchain than Linux. The toolchain can be obtained with:

wget www.antmicro.com/downloads/riscv-gcc.tar.gz

## Getting the sources

```
git clone https://github.com/antmicro/litex-rv32-linux-system
cd litex-rv32-linux-system
git submodule update --init --recursive
```

# U-Boot

U-Boot sources can be obtained with:

```
git clone https://github.com/antmicro/RV32_U-Boot
cd RV32_U-Boot
git checkout 32bit
```

## Building

```
make rv32_defconfig
make -j`nproc`
```

# Ideas

* run the LiteX SoC in Verilator and describe what you needed to do, try to run the Linux there
* add storage drivers in U-Boot for the SPI Flash
* same but for Linux
* route LiteEth (a LiteX IP) to the Ethernet on the board and implement support for it in the LiteX BIOS
* same but for U-Boot
* same but for Linux
* run Zephyr on the LiteX SoC (our Zephyr port is https://github.com/antmicro/zephyr/tree/litex-vexriscv, should be straightforward)
* add support for the weather and luminosity shields as Zephyr drivers
* add functional models for LiteEth, LiteI2C or LiteSPI for Renode
* add your own via a pull request!

