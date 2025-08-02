   ***TASK 1- RISCV TOOLCHAIN SETUP TASKS AND UNIQUENESS SETUP***
                                                                          
 Task 1: 

Base Developer Tools Installation
Compilers,linkers,autotools and libraries required by the RISC‐V simulator, proxy kernel, and other tooling.

       sudo apt-get install -y git vim autoconf automake autotools-dev curl \
       libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
       texinfo gperf libtool patchutils bc zlib1g-dev libexpat1-dev gtkwave
Task 2:
Clean workspace creation and homepath capturing to keep everything contained in ~/riscv_toolchain.
       
          sudo cd 
          pwd=$PWD
          mkdir -p riscv_toolchain
          cd riscv_toolchain 
Task 3:
Prebuilt RISCV Toolchain Installation
riscv64-unknown-elf-gcc (newlib) to compile bare‐metal/user‐space RISC‐V programs.


        wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"

        tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz

Task 4:
Adding Toolchains to the path.

        export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH
        echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH' >> ~/.bashrc
        source ~/.bashrc
        
Task 5:
Installation of Device Tree Compiler(DTC). 

       sudo apt-get install -y device-tree-compiler

Task 6:
Installation of RISCV ISA Simulator - SPIKE

       cd $pwd/riscv_toolchain
       git clone https://github.com/riscv/riscv-isa-sim.git
       cd riscv-isa-sim
       mkdir -p build && cd build

      ../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14

      make -j$(nproc)
      sudo make install

Task 7:  

    git clone https://github.com/riscv/riscv-pk.git
    cd riscv-pk
    git checkout v1.0.0 
    mkdir -p build && cd build  
    ../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf  
    make -j$(nproc)
    sudo make install
    cd $pwd/riscv_toolchain
  
   
   
Task 8:
Ensure the cross bin directory is in PATH  

     echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH' >> ~/.bashrc
     source ~/.bashrc
  ****FINAL DELIVERABLE****    
  Command for compiling unique_test.c    
  
  
     riscv64-unknown-elf-gcc -O2 -Wall -march=rv64imac -mabi=lp64 -DUSERNAME="\"$(id -un)\"" -DHOSTNAME="\"$(hostname -s)\"" unique_test.c -o unique_test
     spike pk ./unique_test
  
  
 <img width="2158" height="187" alt="Screenshot from 2025-08-02 16-08-09" src="https://github.com/user-attachments/assets/6f37cade-f955-48d3-ba1f-4b9248520aae" />
<img width="434" height="138" alt="Screenshot from 2025-08-02 16-08-24" src="https://github.com/user-attachments/assets/462883ab-3d6b-4605-a9da-13f383e28d95" />


 
 


