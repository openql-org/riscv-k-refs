# riscv-k-refs
Reference implementation for the proposed RISC-V K quantum extension

### Prerequisites

Several standard packages are needed to build the toolchain.  On Ubuntu,
executing the following command should suffice:

    $ sudo apt install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev liboscpack-dev


Clone source code recursively. 

    $ git clone --recursive https://github.com/openql-org/riscv-k-refs.git -b develop-openql

NOTE: Need to specify the branch name as a parameter.

Set the toolchain install directory to RISCV environment parameter.

   $ export RISCV=/path/to/install/riscv/toolchain


## Build riscv-gnu-toolchain

    $ pushd riscv-gnu-toolchain
    $ mkdir build
    $ cd build
    $ CC= CXX= ../configure --prefix=${RISCV} --enable-multilib
    
    # For Newlib 
    $ make -j`nproc`
    
    # For Linux 
    $ make -j`nproc` linux
    
    $ sudo make install
    $ popd


## Build riscv-tools

Set the toolchain installed directory to PATH environment parameter.

   $ export PATH=${RISCV}/bin:${PATH}

### Build QuEST library

    $ pushd riscv-tools/QuEST
    $ mkdir build
    $ cd build
    $ cmake ..
    $ make
    
    $ popd

### Make opcode

    $ pushd riscv-tools/riscv-opcode
    $ make
    $ popd

### Build riscv-pk and install

    $ pushd riscv-tools/riscv-pk
    $ autoconf
    $ mkdir build
    $ cd build
    $ ../configure  --prefix=${RISCV} --host=riscv64-unknown-linux-gnu
    $ make
    $ sudo make install
    $ popd

### Build Spike riscv simulator and install

    $ pushd riscv-tools/riscv-isa-sim
    $ autoconf
    $ mkdir build
    $ cd build
    $ ../configure  --prefix=${RISCV}
    $ make CPPFLAGS=-DQUEST -j`nproc`
    $ sudo make install
    $ popd

