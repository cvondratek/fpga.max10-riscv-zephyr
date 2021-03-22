

# attempt #2 - litex + vexriscv
Following zephyr litex_vexriscv board docs @ https://docs.zephyrproject.org/latest/boards/riscv/litex_vexriscv/doc/index.html

## setup
    wget https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14.tar.gz
    tar -xf riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14.tar.gz
    export PATH=$PATH:$PWD/riscv64-unknown-elf-gcc-8.1.0-2019.01.0-x86_64-linux-ubuntu14/bin/

    wget https://raw.githubusercontent.com/enjoy-digital/litex/master/litex_setup.py
    chmod +x litex_setup.py
    ./litex_setup.py init
    sudo ./litex_setup.py install

## Build
    cd litex-wrkspc/litex-boards/litex_boards/targets
    ./de10lite.py --cpu-type vexriscv --build


# attempt #1 - fusesoc + picorv32
Builds for de0 without issue but picorv32 lacks PLIC which is currently required for zephyr risc-v

Background: https://github.com/micro-FPGA/riscv-contest-2018/issues/3

Abandoning this path for now

## setup & test fusesoc
git clone https://github.com/olofk/fusesoc.git
cd fusesoc
pip3 install --user .
mkdir workspace
cd workspace
export PATH=$HOME/.local/bin:$PATH
fusesoc library add fusesoc-cores https://github.com/fusesoc/fusesoc-cores
fusesoc core list
cd -

## prep 
sudo apt-get install iverilog gtkwave libelf-dev

fusesoc --cores-root cores/ --target=sim picorv32-wb-soc
gtkwave build/picorv32-wb-soc_0/sim-icarus/picorv32-wb-soc.vcd


## last working build
fusesoc --cores-root cores/ run --tool=quartus de0-nano-picorv32-wb-soc


# Altera USB Blaster Programming

cd usr/lib/x86_64-linux-gnu
sudo ln -s libudev.so.1 libudev.so.0

/etc/udev/rules/37-usbblaster.rules:

SUBSYSTEM=="usb", ATTRS{idVendor}=="09fb", ATTRS{idProduct}=="6001", GROUP="plugdev", MODE="0666",SYMLINK+="usbblaster"

restart udev


