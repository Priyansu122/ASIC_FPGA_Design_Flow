# ASIC_FPGA_Design_Flow
This page will contain the steps to install the tools required for both [FPGA](#FPGA_FLOW) and ASIC flow and the steps to execute the flow in digital design.
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
  designers ensure that circuits meet timing requirements by calculating the delay of signals through various paths in the design
  


      
      
      
    


  
  ## FPGA_FLOW
