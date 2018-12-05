# Running Linux on LiteX based Risc-V SoC

# Dependencies on Ubuntu 18.04
Install dependencies with
```
sudo apt install device-tree-compiler bison flex
```

You will also need to run this to add a symlink for this library:
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpfr.so.6 /usr/lib/x86_64-linux-gnu/libmpfr.so.4
```

Edit your `~/.bashrc` file and add these two lines to the end.  Make sure to replace with your actual path:

```
export PATH=$PATH:/path/to/riscv-gnu-toolchain/bin
export PATH=$PATH:/path/to/riscv-gcc/bin
```

Close and re-open your bash window to load these new variables.

# Getting the toolchain

```
wget www.antmicro.com/downloads/riscv-gnu-toolchain.tar.gz
```

# Getting the sources

```
git clone https://github.com/antmicro/litex-linux-riscv.git
```

# Building

kernel configuration:

```
cd litex-linux-riscv
cp litex_default_configuration .config
ARCH=riscv CROSS_COMPILE=riscv32-unknown-linux-gnu- make -j`nproc`
riscv32-unknown-linux-gnu-objcopy -O binary vmlinux vmlinux.bin
```

devicetree:

```
dtc -I dts -O dtb -o vmlinux.dtb rv32.dts
```

# Litex SoC

Litex requires different gcc toolchain than Linux. The toolchain can be obtained with:

```
wget www.antmicro.com/downloads/riscv-gcc.tar.gz
```

## Getting the sources

```
git clone https://github.com/antmicro/litex-rv32-linux-system
cd litex-rv32-linux-system
git submodule update --init --recursive
```

# Building the SoC

```
source env.sh
# to generate verilog run
make verilog
# to generate verilog, build the system and generate programming file run (this step requires microsemi tools intalled)
make
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

# openOCD

Getting the sources:

```
git clone https://github.com/SpinalHDL/openocd_riscv
```

building

```
./configure --enable-ftdi --enable-dummy --disable-werror
make -j`nproc`
sudo make install
```

connecting to the board

```
openocd -f vexriscv.cfg

```

in a separate terminal run telnet and connect to the openOCD

```
telnet localhost 4444
```

upload and run Linux:

```
halt
load_image /path/to/vmlinux.bin 0x40000000
load_image /path/to/vmlinux.dtb 0x41000000
load_image /path/to/initramdisk.gz 0x42000000
resume 0xC0000000
```

# Renode

To run Renode go to http://github.com/renode/renode and get the vexriscv branch.

After you compile it, open the `scripts/hackaton` file and adjust the paths to the vmlinux, dtb and rootfs files.

Then start Renode and run:

```
s @scripts/hackaton
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

