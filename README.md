# SureLog tool example 

## Buld dependencies

## Build and install

```bash
git clone ...
cd surelog-tool-example 
git submodule update --init --recursive
mkdir Build
cd Build
cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=`pwd`/install
make install -j8
```
