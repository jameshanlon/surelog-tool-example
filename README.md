# Verilog Netlist

## Buld dependencies

## Build and install

```bash
git clone ...
cd verilog-netlist
git submodule update --init --recursive
mkdir Build
cd Build
cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=`pwd`/install
make install -j8
```
