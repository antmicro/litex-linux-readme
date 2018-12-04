# Running Linux on LiteX based Risc-V SoC

# Getting the sources

`git clone https://github.com/antmicro/litex-linux-riscv.git`

# Building

kernel configuration:

cd litex-linux-riscv
cp litex_default_configuration .config
ARCH=riscv CROSS_COMPILE=CROSS_COMPILE=riscv32-unknown-linux-gnu- make -j`nproc`
riscv32-unknown-linux-gnu-objcopy -O binary vmlinux vmlinux.bin

devicetree:

dtc -I dts -O dtb -o vmlinux.dtb rv32.dts

