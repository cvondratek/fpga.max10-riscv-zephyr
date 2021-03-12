# setup & test fusesoc
git clone https://github.com/olofk/fusesoc.git
cd fusesoc
pip3 install --user .
mkdir workspace
cd workspace
export PATH=$HOME/.local/bin:$PATH
fusesoc library add fusesoc-cores https://github.com/fusesoc/fusesoc-cores
fusesoc core list
cd -

# prep 
sudo apt-get install iverilog gtkwave libelf-dev

fusesoc --cores-root cores/ --target=sim picorv32-wb-soc
gtkwave build/picorv32-wb-soc_0/sim-icarus/picorv32-wb-soc.vcd


# works
fusesoc --cores-root cores/ run --tool=quartus de0-nano-picorv32-wb-soc


