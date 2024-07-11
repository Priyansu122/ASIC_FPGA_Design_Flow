# ASIC_FPGA_Design_Flow
This page will contain the steps to install the tools required for both [FPGA](#FPGA_FLOW) and [ASIC](#ASIC_FLOW) Flow and the steps to execute the flow in digital design.
## Tools required
- For FPGA flow we need tools like</br>
  1. Iverilog and gtkwave - For simulation and Functional verification
  2. Yosys - for synthesis
  3. NextPnr - For Place and route
  4. Project IceStorm - tools for working with Lattice ICE40 bitstreams
- For ASIC Flow we need tools like</br>
  1. Iverilog and gtkwave - For simulation and Functional verification
  2. Yosys - for synthesis
  3. OpenSTA - For static time analysis report
  4. OpenRoad - For Physical design
     
- We will be discussing about installation of the tools and steps to use them in the subsequent sections.
## Installation

  ### OSS CAD SUIT
  The OSS CAD Suite is a collection of open-source tools for electronic design automation (EDA). It integrates several powerful tools into one package, making it easy to work on various stages of digital 
  design, from writing HDL code to simulating and verifying your circuits.
  - **Included Tools** : Tool that you will need to execute the flow</br>
     - Iverilog : Verilog compilation system
     - Gtkwave : fully featured GTK+ based wave viewer
     - Yosys: A tool for synthesizing Verilog designs.
     - NextPNR: A place-and-route tool for FPGAs.
     - Project IceStorm : tools for working with Lattice ICE40 bitstreams (Specifically required to build with Icebreaker FPGA board)
  - **Features**</br>
    - All-in-One: Install multiple EDA tools at once.
    - Open-Source: Free to use, modify, and distribute.
    - Cross-Platform: Available for Windows, Linux, and macOS.
  - **Installation : For Windows**</br>
  Before starting update and upgrade using the following command in the WSL window. </br>
  **sudo apt update </br>
  sudo apt upgrade**



    - Step1 : Download an archive matching your OS from [the releases page](https://github.com/YosysHQ/oss-cad-suite-build/releases/latest).
    - Step2 : Extract the compressed file in your desired location </br>
      - Example : Say you want to extract the file in D drive in your system then you have to mount to D drive in WSL then extract using "tar" command
      - create a Folder in D drive and execute these commands one by one:</br>
        ```bash
        cd /mnt/d
        mkdir cadToolFolder
        cd cadToolFolder/
        tar -xvzf oss-cad-suite-linux-x64-20240620.tgz -C /mnt/d/cadToolFolder/
        ```
    - Step3 : set the enviroment </br>
      - Open your bashrc file :
        ```bash
         vim ~/.bashrc
        ```
      - Paste the following line :
        ```bash
        export PATH="/mnt/d/cadToolFolder/oss-cad-suite/bin:$PATH"
        ```
      - Source the bashrc file
        ```bash
        source ~/.bashrc
        ```
      - To confirm installation, run a command such as ```nextpnr-ice40``` or ```yosys```. Now You are ready with the tools that OSS CAD SUIT contains.
- **Note**</br>
      All installation information provided in this document is sourced from the OSS CAD Suite repository by YosysHQ. For more detailed information and updates, please visit their
      [GitHub page](https://github.com/YosysHQ/oss-cad-suite-build.git).

  ### OpenSTA
  OpenSTA (Open Static Timing Analyzer) is an open-source tool used in digital integrated circuit design to analyze the timing of digital circuits. It helps 
  designers ensure that circuits meet timing requirements by calculating the delay of signals through various paths in the design.

  **Installation steps [For Windows]** </br>
  Step1 : Prerequisites </br>
  The build dependency versions are shown below. Other versions may work, but these are the versions used for development.
```bash
Dependacies   Versions   How to check if installed 
-----------   --------   -------------------------
cmake         3.24.2     cmake --version
clang                    clang --version
gcc           11.4.0     gcc --version
tcl           8.6        dpkg -l | grep tcl
swig          4.1.0      swig -version
bison         3.8.2      bison --version
flex          2.6.4      flex --version
```
External library dependencies:</br>
```bash
         
eigen            3.4.0    dpkg -l | grep libeigen
tclreadline               dpkg -l | grep tclreadline
libz             1.2.5    dpkg -l | grep libz
cudd             2.4.1    ls /usr/local/lib | grep libcudd
                          ls /usr/local/include | grep cudd
```
Step 2 : Installation --> Write and execute the following commands in WSL terminal one by one
```bash
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git 
cd OpenSTA 
mkdir build
cd build
cmake .. 
make
sudo make install
```
step 3 : Verify the installation by using the following command
```bash
sta
```
**Note** :
1. If any dependencies are missing, you can use ChatGPT for assistance in installing them on your system. 
2. All build information is sourced from the OpenSTA GitHub repository. Visit for more details.

### OpenRoad
OpenROAD is a comprehensive tool for chip physical design that transforms a synthesized Verilog netlist into a routed layout.

The OpenROAD Project has two releases:

- Application [(github)](https://github.com/The-OpenROAD-Project/OpenROAD) [(docs)](https://openroad.readthedocs.io/en/latest/main/README.html): The application is a standalone binary for digital place and route that can be used by any other RTL-GDSII flow controller.
- Flow [(github)](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts) [(docs)](https://openroad-flow-scripts.readthedocs.io/en/latest/): This is the native OpenROAD flow that consists of a set of integrated scripts for an autonomous RTL-GDSII flow using OpenROAD and other open-source tools.
- We will be following the application github page becuase it allows us to divide the whole script to subscripts like script for floorplanning, script for detailed routing hence that will give us a better idea on flow and will help us to visualize
- You can explore flow github release for Automated RTL to GDS flow

  **Installation**
  - Follow the following steps that is execute following commands one by one in the WSL terminal
  ```bash
  git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git
  cd OpenROAD
  sudo ./etc/DependencyInstaller.sh
  mkdir build
  cd build
  cmake
  make
  sudo make install
  ```
  - Verify the installation using the following command
  ```bash
  openroad
  ```

## ASIC_FLOW  
- This section will describe the RTL to GDS flow step by step. By following these steps you can converts your design to the final layout file
that will be given to Fabrication house.
- Here we will be using [sky130 technology library](https://skywater-pdk.readthedocs.io/en/main/contents/libraries.html).
- A technology library is provided by the foundry because it contains standardized, pre-characterized cells and components specifically designed for a particular semiconductor manufacturing process. These libraries ensure that designs are optimized for performance, power, and area, and they help designers create reliable and manufacturable integrated circuits (ICs) by providing accurate models and parameters that match the foundry's process capabilities

### STEPS
- Step1 : **Design and Verification** </br>
  - Creat a file say counter.v for your design and another file say tb_counter.v for the testbench.
  - Execute the following command,
```bash
iverilog -o counter counter.v tb_counter.v
vvp counter
```
--> if you have any syntax error in your design you can see that after executing 1st command </br>
--> After executing 2nd command a .vcd file will be generated </br>
**Note** : The name of the .vcd file will be same as you have given in the testbench. for example : $dumpfile("test.vcd")
Then execute the following command 
```bash
gtkwave test.vcd
```
--> You can see the waveform and verify your design functionality

- Step2 : **Synthesis using Yosys**
    - Synthesis required basically 2 files i.e design(.v file), Technology library(.lib file).
    - Synthesis of a design consist of several commands.
    - we will create a tcl script i.e a file named say synthesis.tcl and we will put all commands there.
    - The commands are as follows, 
```bash
# read design
read_verilog counter.v

# elaborate design hierarchy
hierarchy -check -top counter

# the high-level stuff
proc; opt; fsm; opt; memory; opt

# mapping to internal cell library
techmap; opt

# mapping flip-flops to mycells.lib
dfflibmap -liberty /home/priyansu122/resources/sky130hd/sky130hd_tt.lib

# mapping logic to mycells.lib
abc -liberty /home/priyansu122/resources/sky130hd/sky130hd_tt.lib

# cleanup
clean

# write synthesized design
write_verilog -noattr synth.v

```
-->  Launch the yosys tool </br>
 $ yosys </br>
--> You can individually run the commands listed above. I am using a tcl file.</br>
 yosys> script synthesis.tcl </br>
--> synth.v is the netlist created by yosys tool.</br>
--> You can observe that the modules used in synth.v is from the technology library.</br>
--> Again you can check the functionality of the synthesized code using follwing commnad.
```bash
iverilog -o counter synth.v tb_counter.v formal_pdk.v
vvp counter
gtkwave test.vcd
```
--> formal_pdk.v is the file which describes the functionality of the modules present in the technology library.</br>
--> You can find this in this location "~/OpenROAD/test/sky130hd/work_around_yosys"

- Step3 : **Static time Analysis**
   - You can use this to findout setup slack and hold slack if both the slacks comes out to be positive in the report then the STA test is passed.
   - 

  
  
  

  
 


