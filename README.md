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

## FPGA_FLOW
