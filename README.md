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

***TASK 2 : RUN DISASSEMBLE DECODE***  

GOALS  

  **To run C programs and compile using Local Toolchain and spike simulator  
  
  **To generate assembly code  
  
  **To decode RISCV integer instructions.  
  
----->>  SPIKE VERSION  


  <img width="695" height="69" alt="Screenshot from 2025-08-02 18-08-17" src="https://github.com/user-attachments/assets/ea944b04-9bf3-4e3d-8886-4f48b221d63e" />   
    
----->> OUTPUT OF  riscv64-unknown-elf-gcc -v     

      riscv64-unknown-elf-gcc -v
  <img width="2478" height="273" alt="elf_version" src="https://github.com/user-attachments/assets/6c1051b3-02ed-424c-a8e0-4d9f46b29d8a" />


  ***Program Implemented : factorial.c***    

      
   
      riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
     -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
      DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
     factorial.c -o factorial 
SPIKE SIMULATOR  
     
     spike pk ./factorial
  
<img width="777" height="336" alt="FACT_UNIQUE" src="https://github.com/user-attachments/assets/e84b4f17-c239-47b6-9e35-7feb2d52001a" />

ASSEMBLY AND DISASSEMBLY   

     riscv64-unknown-elf-gcc -O0 -S factorial.c -o factorial.s
     riscv64-unknown-elf-objdump -d ./factorial | sed -n '/<main>:/,/^$/p' | tee factorial_main_objdump.txt 
  <img width="1558" height="611" alt="DISASSEMBLY" src="https://github.com/user-attachments/assets/b7d15494-36cc-45ad-8c17-38c33a1556dc" />


***Program Implemented: max_array.c

      
 


