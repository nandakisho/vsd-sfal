# vsd-sfal
<details>
<summary>
Install yosys, iverilog and gtkwave </summary>
1.1: install yosys

    $ sudo apt-get update


    $ git clone https://github.com/YosysHQ/yosys.git


    $ cd yosys


    $ sudo apt install make (If make is not installed please install it) 


    $ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev

    
    $ make config-gcc


    $ make 


    $ sudo make instal


yosys:


![Screenshot from 2024-07-23 02-54-34](https://github.com/user-attachments/assets/61eb255c-02e2-408a-8fcf-77741d4e4a8f)


1.2: install icarus-iverilog


    sudo apt-get update


    sudo apt-get install iverilog


![Screenshot from 2024-07-23 02-51-13](https://github.com/user-attachments/assets/2141a391-6055-44f6-965e-6fbb0829239e)


1.3: install gtkwave


    sudo apt-get update


    sudo apt install gtkwave


![Screenshot from 2024-07-23 02-56-14](https://github.com/user-attachments/assets/b135e337-7015-41c2-98a5-b90ea0192c4a)


1.4: Iverilog:


Used to verify the rtl code for basic gates, adders, multipliers, adders etc. 
All the verilog codes have assosciated Test bench. 
Iverilog uses two arguments to dump a intermediate a.out file, which can be run in terminal to get the the VCD file.
VCD: Value change dump
     
 ![Screenshot from 2024-07-23 03-01-24](https://github.com/user-attachments/assets/060e9b70-54c1-46b8-95a4-5f03f40a3157)


vcd file is launched using gtkwave to verify the waveform:


![Screenshot from 2024-07-23 23-46-41](https://github.com/user-attachments/assets/20fd89c5-7d97-451e-a639-ca7bff8639d9)
![Screenshot from 2024-07-23 23-47-33](https://github.com/user-attachments/assets/294f982d-200d-4c10-93a4-18c72ba35c47)

     
Snippet from Test Bench to verify the output of counter:

![Screenshot from 2024-07-23 23-51-20](https://github.com/user-attachments/assets/bb801a3e-9d40-4f04-bcce-cec7e3b95482)

YOSYS: The open source synthesizer tool

![Screenshot from 2024-07-23 02-54-34](https://github.com/user-attachments/assets/78bd9e4a-dd80-4830-8b0e-3d92c5e3b760)

commands used: in order

Read Liberty: The Infamous .lib file. Lib used here is sky130 typical library. P: Typical T:25 V:1.8v

    read_liberty -lib ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  

Read verilog: path to vrilog file mentioned here

     read_verilog ../../../sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_counter.v 

Yosys command to synthesize the deign under test to generic tech, irrespective og the sky130nm tech

    synth -top good_counter 

Tech mapping to sky130nm cells, This gives us data on what kind of cells were handpicked from sky130 lib

    abc -liberty ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

SHOW: command use to view the dumped schematic/logic, in its Heirachical or flat nature

    show 

Write the gate level netlist (Heirarchical netlist), which can be used further for Placement and Route activites.

    write_verilog -noattr good_counter_ghnetlist.v

Flatten: Command used to get flat netlist.

    flatten

Write out the gate level netlist for the flatten netlist

    write_verilog -noattr good_counter_gfnetlist.v

![Screenshot from 2024-07-23 03-08-18](https://github.com/user-attachments/assets/3403b482-1a57-475e-b0f9-608b9d7f68e5)
![Screenshot from 2024-07-23 03-09-22](https://github.com/user-attachments/assets/9b2c5335-a327-4013-b207-15621caee607)
![Screenshot from 2024-07-23 03-10-48](https://github.com/user-attachments/assets/0473cccc-a0cc-4b0d-9c16-970ab71611a6)
![Screenshot from 2024-07-23 03-11-16](https://github.com/user-attachments/assets/1e0ade3a-f3b8-465e-9bd0-13d044e67610)
![Screenshot from 2024-07-24 00-07-22](https://github.com/user-attachments/assets/04a88079-4884-47f3-907d-3a94e4c3900d)


Synthesized netlist:


![image](https://github.com/user-attachments/assets/0477bef7-a001-48e7-aef5-049b15a372b9)


Sky130 lib snippet:


![Screenshot from 2024-07-25 00-41-18](https://github.com/user-attachments/assets/82ace1d9-7a33-45c3-a440-0e404f70bd35)


Hierarchy vs Flat netlist: .v file used: multiple_modules.v


    read_liberty -lib ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    read_verilog ../../../sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v 
    synth -top multiple_modules 
    show multiple_modules 
    abc -liberty ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    

synth -top multiple_modules: return defaults Heirarchy enabled design.

![Screenshot from 2024-07-25 02-04-32](https://github.com/user-attachments/assets/754cb398-32e9-48a0-a4a2-20554d602049)


flatten: command used to get Flat netlist

![Screenshot from 2024-07-25 02-08-17](https://github.com/user-attachments/assets/ed1ff579-4fd1-4386-814f-e3a0590bdf98)

Submodule level synthesis:
Need: 

To instantisate "identical modules" multiple times in a design, instaed of overloading the tool, that can break due to the code repitition.
The synthesized module can be instantiated as many times as we need and stich them in top file.

synth -top sub_module1:

![Screenshot from 2024-07-25 02-27-55](https://github.com/user-attachments/assets/e82d0a6e-c3fd-4738-9c2b-e8cc385e6934)

![Screenshot from 2024-07-25 02-21-28](https://github.com/user-attachments/assets/685592bc-48ed-4236-93c2-37ed1ed89d72)


synth -top sub_module2:


![Screenshot from 2024-07-25 02-28-12](https://github.com/user-attachments/assets/27f6d605-0778-4b67-b52d-2775d0d3ae29)

![Screenshot from 2024-07-25 02-22-43](https://github.com/user-attachments/assets/9bc86156-5be5-47f5-a93f-180062f89ce2)


# DFF usage in Yosys.
## DFF with asynchronous reset:
verilog snippet:

![Screenshot from 2024-07-27 21-45-30](https://github.com/user-attachments/assets/f29aa735-59c7-4915-aa38-cb9d9e5a4c38)


Waveform:

![Screenshot from 2024-07-27 18-02-34](https://github.com/user-attachments/assets/53a9ec55-f3b5-4c50-9702-a23afa80dc21)

show:
![Screenshot from 2024-07-27 22-07-29](https://github.com/user-attachments/assets/3acd41b7-644b-43eb-8fcd-be06ab0cbfd3)

synthesized netlist:
![Screenshot from 2024-07-27 22-10-25](https://github.com/user-attachments/assets/df723a86-442d-4ed6-9653-056e0b292db2)



## DFF with Asynchronouts set

Verilog snippet:

![Screenshot from 2024-07-27 21-45-55](https://github.com/user-attachments/assets/c82714ec-89a5-4acc-9f68-12eec14dfaba)

Waveform:

![Screenshot from 2024-07-27 18-07-07](https://github.com/user-attachments/assets/78b0b162-b275-4c95-be61-36733492e0a0)

show:

![Screenshot from 2024-07-27 22-13-35](https://github.com/user-attachments/assets/36f77fd8-6e36-4839-abb4-5e608d541ffb)

Synthesized netlist:

![Screenshot from 2024-07-27 22-15-18](https://github.com/user-attachments/assets/031a35e7-660b-4811-9819-d8e8c95c85dc)




# Combination and Sequential Optimizations:
## Topics like: Constant propagation, Boolean optimisation, State Optimisation, Retiming, Logic Cloning were explained with basic combinational and DFF gates.

# Opt Lab
To understand the implementation of basic Ternary statements into logic gates by Yosys.

Verilog snippet:

![Screenshot from 2024-07-27 22-32-09](https://github.com/user-attachments/assets/7394f217-818e-4787-b59f-2d8da384f607)

Basically a ternary expression for AND gate: Yosys implementing and gate with Opt_clean -purge

![Screenshot from 2024-07-27 22-31-07](https://github.com/user-attachments/assets/473849e9-bea9-4157-bda1-4dcf2f94b25c)

commands used:

![Screenshot from 2024-07-27 22-33-56](https://github.com/user-attachments/assets/a4de0ffe-fd1b-4738-a2b7-886958038be7)

# Opt check 2

Verilog snippet:

![Screenshot from 2024-07-27 22-48-31](https://github.com/user-attachments/assets/866b5a1a-5e03-463f-bb7b-e49ba7fc7903)


OR gate Implemented:


show: before

    opt_purge -clean

![Screenshot from 2024-07-27 22-40-36](https://github.com/user-attachments/assets/cd1ac00e-d776-4280-be68-f02e32715a3e)

After:    
    Opt_clean -purge
    
    
show:

![Screenshot from 2024-07-27 22-42-03](https://github.com/user-attachments/assets/76c79d4c-96c8-4bff-81c6-346a764e9ca8)


Stats: 

before mapping

![Screenshot from 2024-07-27 22-40-19](https://github.com/user-attachments/assets/234fe7cc-24b9-4812-b0ba-6e5816682557)


Stats: 

after mapping

![Screenshot from 2024-07-27 22-41-27](https://github.com/user-attachments/assets/03d960d1-3883-4388-8417-7c33c67799cf)

# 3 input and gate

verilog snippet:

![Screenshot from 2024-07-27 22-51-50](https://github.com/user-attachments/assets/97052fa5-9297-4c5b-bb5b-649b1ed03069)

abc mapped:

![Screenshot from 2024-07-27 22-51-17](https://github.com/user-attachments/assets/e0697802-85b7-426f-af5d-2999dbe69c39)


# Sequential Optimization:

Verilog snippet:

D Flip Flop implemeted due to Q toggling

![Screenshot from 2024-07-27 23-35-10](https://github.com/user-attachments/assets/6fafcf37-ef69-4e66-8b39-69fe528f11f4)

show:

![Screenshot from 2024-07-27 23-29-37](https://github.com/user-attachments/assets/30a592ed-adde-4848-8a17-b11ac6c6c892)


gate level netlist:

![Screenshot from 2024-07-27 23-38-10](https://github.com/user-attachments/assets/a9a1cff7-fb5c-4a55-8e47-e75ffafcca1e)


Verilog snippet:

DFF not inferred due to tie high logic equivalent circuit.

![Screenshot from 2024-07-27 23-34-35](https://github.com/user-attachments/assets/fedefd3b-9abb-45b0-86cb-732b894365d5)



show:

![Screenshot from 2024-07-27 23-31-22](https://github.com/user-attachments/assets/ff6dd6a6-c518-402e-93f5-0e3cfba21207)


gate level netlist:

![Screenshot from 2024-07-27 23-34-35](https://github.com/user-attachments/assets/750179f8-6e79-4b1f-b36c-d9801178083c)


DFF const 3: The tricky Flop: Reset and Set flop with Reset and clock shorted. A small glitch can be observed in gtkwave simulation.
This can break the circuit functionality, if not handled carefully.

verilog snippet:

![Screenshot from 2024-07-27 23-54-32](https://github.com/user-attachments/assets/443e3290-9f78-4f3e-866b-25450ebb6810)


gtkwave simualtion:

![Screenshot from 2024-07-28 00-11-26](https://github.com/user-attachments/assets/9fd415d7-cce7-4829-90e1-53f0fdfcd00a)



show:

![Screenshot from 2024-07-27 23-49-57](https://github.com/user-attachments/assets/0ad89813-ffbe-4a33-9e1b-a2db413d3f53)


gate level netlist:

![Screenshot from 2024-07-27 23-51-24](https://github.com/user-attachments/assets/3fa31271-e753-46b9-859c-bf029b608dfd)

</details>

<details>
<summary> GLS: Gate level Simulation </summary>
    
## caveats observed in verilog coding that will affect GLS mismtach.
1. sensitivity level mismtach
2. blocking and non blocking mismatch
3. non standandard verilog coding

## Sensitivity level mismtach

![Screenshot from 2024-07-29 20-12-09](https://github.com/user-attachments/assets/6cba1d83-68c2-4f18-b643-c6402450a7a0)

## Blocking and non blocking statements

![Screenshot from 2024-07-29 20-13-38](https://github.com/user-attachments/assets/a0ca6b6c-98e4-4a31-a7b5-94b3d22be058)
![Screenshot from 2024-07-29 20-14-46](https://github.com/user-attachments/assets/f05a6916-27ee-46ff-ab41-bd06d6692840)
![Screenshot from 2024-07-29 20-15-22](https://github.com/user-attachments/assets/13786a92-e1c3-4f29-9f11-009e7306b338)


## Good Simulation match between: verilog and gate level netlist

Verilog Snippet:

![Screenshot from 2024-07-29 20-21-06](https://github.com/user-attachments/assets/03318624-3867-4e9f-8e7a-458707363292)


gtkwave simulation:

![Screenshot from 2024-07-29 20-20-15](https://github.com/user-attachments/assets/d7512a46-239a-4c8b-b304-d9ed13938124)



gate level netlist:

![Screenshot from 2024-07-29 20-21-50](https://github.com/user-attachments/assets/a6cbe528-f675-477d-a309-9ab7c0795711)



gtkwave simulation:
![Screenshot from 2024-07-29 20-24-54](https://github.com/user-attachments/assets/60214645-39fa-4fd5-abe0-b21c059545fd)



commands used:

Yosys:

    read_liberty -lib ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    read_verilog ../../../sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v 
    synth -top good_mux 
    abc -liberty ../../../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    show 
    write_verilog -noattr good_mux_gnetlist.v

iverilog and gtkwave commands used:

    iverilog ../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v ../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v ../yosys/projects/gls_lab/good_mux_gnetlist.v         ../sky130RTLDesignAndSynthesisWorkshop/verilog_files/tb_good_mux.v

gtkwave tb_good_mux.vcd


## Simulation mismatch: due to sensitivity mismatch

verilog snippet:

![Screenshot from 2024-07-29 20-27-13](https://github.com/user-attachments/assets/0c69ea6d-024c-4df5-adf2-f72745af21ef)



gtkwave simulation:

![Screenshot from 2024-07-29 20-29-34](https://github.com/user-attachments/assets/3b560420-6417-4cd6-ba4f-ec5275d1019e)



gate level netlist:

![Screenshot from 2024-07-29 20-32-10](https://github.com/user-attachments/assets/3f22ff1f-4bf1-4fb6-a2f7-f685b29e2f1d)




gtkwave simulation:

![Screenshot from 2024-07-29 20-34-42](https://github.com/user-attachments/assets/8e6db773-deae-4d51-b14a-2296bffdf66d)



yosys:

![Screenshot from 2024-07-29 20-30-55](https://github.com/user-attachments/assets/26da1829-5753-4f01-aee2-000401932e07)


</details>

<details>

<summary>
DC: SYNOPSYS DESIGN COMPILER</summary>

## Sythesis tool: Used to convert RTL to gate level netlist.

![Screenshot 2024-07-29 at 10 52 06 PM](https://github.com/user-attachments/assets/3c27aa9d-e74b-4655-b5a1-e8e0d0f2e197)
Source: UC Davis


### Inputs to DC:
.lib/.db: Sky130 file
.SDC: Synopsys Constraint file.
.V: RTL netlist

### Outputs:
Synthesis Qor
gate level netlist
ddc file.

Lab1: Flop with Latch enable

Commands used:
csh #to enter C Shell

    DC_SHELL #to open Design compiler

![Screenshot 2024-07-29 at 11 00 42 PM](https://github.com/user-attachments/assets/94e13719-03e0-4572-9fae-e84fc9769a98)

.synopsys_dc.setup file: to initialize the libraries

#setting target and link library to defualy sky 130nm db#

    set target_library ../lib/sky130_fd_sc_hd__tt_025C_1v80.db
    set link_library { * ../lib/sky130_fd_sc_hd__tt_025C_1v80.db}

commands to write verilog and ddc file:

![Screenshot 2024-07-29 at 11 05 41 PM](https://github.com/user-attachments/assets/d99cc841-de69-4d8a-b231-a72b859f9726)


gate level netlist:

![Screenshot 2024-07-29 at 11 04 53 PM](https://github.com/user-attachments/assets/fcfe4224-d749-4136-af1e-17a2b8cdc92b)

# Design Vision: 
## it is the gui version used to load the ddc file to view the schematic.

commands used:

    csh
    design_vision
    read_ddc lab1.ddc

![Screenshot 2024-07-29 at 11 11 06 PM](https://github.com/user-attachments/assets/7215e8a1-6364-435e-95b3-db7aff6f530a)

schematic:

abstract:

![Screenshot 2024-07-29 at 11 12 54 PM](https://github.com/user-attachments/assets/a0f45812-8c58-45c4-86cf-db6fb263224d)


Expand Heirarchy:

![Screenshot 2024-07-29 at 11 13 02 PM](https://github.com/user-attachments/assets/5916cb26-46e4-45fb-b2f4-a588fc762c43)


# Synopsys tools works with TCL language:

## using foreach collection to loop thru all the cells used in library:

![Screenshot 2024-07-29 at 11 18 37 PM](https://github.com/user-attachments/assets/594aa0b3-2f05-408d-b18b-913e88e780f8)

</details>

<details>

<summary>
STA: Static Timing Analysis</summary>

what is setup time:
The min time for which the data needs to be stable before the clock edge.

<img width="1301" alt="Screenshot 2024-07-29 at 11 23 06 PM" src="https://github.com/user-attachments/assets/1d86c109-09f0-4571-a5fc-af0a8e62f81b">



What is hold time:
The min time the data needs to be stable after the clock edge.

<img width="1040" alt="Screenshot 2024-07-29 at 11 27 44 PM" src="https://github.com/user-attachments/assets/92580455-c343-40e9-a21c-adced4d4d0ad">

Delay of a cell:

<img width="1479" alt="Screenshot 2024-07-29 at 11 35 21 PM" src="https://github.com/user-attachments/assets/655c9393-f1a2-4bbf-99cb-16dc4acf83fa">

Timing arcs:

Combinational gates:

<img width="1393" alt="Screenshot 2024-07-29 at 11 39 07 PM" src="https://github.com/user-attachments/assets/dea11afe-090f-40e3-a680-1839c5b97b6d">



Sequential gates:

<img width="1479" alt="Screenshot 2024-07-29 at 11 39 35 PM" src="https://github.com/user-attachments/assets/93005cb4-946a-4bc1-8c66-03144be9809b">

</details>

<details>

<summary>
CONSTRAINING A DESIGN:</summary>

## Snippet for Constaining a design:

<img width="1656" alt="Screenshot 2024-07-30 at 12 09 55 PM" src="https://github.com/user-attachments/assets/9eca2ead-0efa-4e80-a8ab-a0ec78e3c2fd">

SDC plays a crucial rule to define the clock parameters to meet timing of a design.

## Timing Paths:
1. IN2REG
2. REG2REG
3. REG2OUT
4. IN2OUT

IO Modelling: IO budgetting is important to model IN2REG and REG2OUT paths.
SDC CLK will constrain all REG2REG paths.

## Important check:
To do Sanity checks for all input files, report Qor after each stage of PnR.

#Summary:

<img width="1656" alt="Screenshot 2024-07-30 at 12 40 07 PM" src="https://github.com/user-attachments/assets/7c850f96-d397-4940-8d87-eb6e33bda985">
<img width="1656" alt="Screenshot 2024-07-30 at 12 44 33 PM" src="https://github.com/user-attachments/assets/d7148e56-cd57-4776-b5f5-8a100f4805d0">

</details>

<details>
<summary> Intoduction to BabySoc</summary>
    
What is SoC?
SoC is a single-die chip that has some different IP cores on it. These IPs could vary from microprocessors (completely digital) to 5G broadband modems (completely analog).
The design of a system on chip usually includes a central processing unit, memory, ports for input and outputs, secondary storage devices, and peripheral interfaces such as Timers, etc.
Depending upon the requirement it can also consist of a digital or analog signal processing system or a floating-point unit.
SoC with equivalent functionality will have increased performance and reduced power consumption as well as a smaller semiconductor die area.


Why SoC?

VSDBabySoC is a small yet powerful RISCV-based SoC. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. VSDBabySoC contains one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.

![image](https://github.com/user-attachments/assets/d7dcc6b8-e7dd-4fed-be88-7991e110a4eb)


source: https://github.com/Devipriya1921/VSDBabySoC_ICC2


Problem statement

This work discusses the different aspects of designing a small SoC based on RVMYTH (a RISCV-based processor). This SoC will leverage a PLL as its clock generator and controller and a 10-bit DAC as a way to talk to the outside world. Other electrical devices with proper analog input like televisions, and mobile phones could manipulate DAC output and provide users with music sound or video frames. At the end of the day, it is possible to use this small fully open-source and well-documented SoC which has been fabricated under Sky130 technology, for educational purposes.

Design elements used in BabySoC:
1.RVMYTH
2.PLL
3.DAC
4.SPI

RVMYTH: 

It is basic RISCV CPU developed by Steve Hovver and VSD sysytem deisgn to comprehend the knowledge of a simple cpu and its working mechanism, it is a 5bit cpu machine.
All these designs are open source and modelled using their git repos, which are open source.

PLL:

The phase-locked loop (PLL) block is a feedback control system that automatically adjusts the phase of a locally generated signal to match the phase of an input signal. PLLs operate by producing an oscillator frequency to match the frequency of an input signal. In 
this locked condition, any slight change in the input signal first appears as a change in phase between the input signal and the oscillator frequency. This phase shift then acts as an error signal to change the frequency of the local PLL oscillator to match the 
input signal. The locking-onto-a-phase relationship between the input signal and the local oscillator accounts for the name phase-locked loop. PLLs are often used in high-speed communication applications.

Source: Intel

DAC: 

It is a Digital to analog converted ubiquitously used across multiple IC domains, they mainly serve in conerting all digital signals to analog and helps with communicating either with external world or Intra chip.

SPI: 

Serial Peripheral Interface it a protocol used for communication of data between devices, a synchronous mechanism with full duplex interface capability, maily serves intra chip.

## Simulating babysoc using Iverilog and gtkwave
###pre synth simulation

    sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
    sudo chmod 666 /var/run/docker.sock
    cd ~
    pip3 install pyyaml click sandpiper-saas #pip3 is a pyhton install application

clone vsdbabysoc.git from repo
    git clone https://github.com/manili/VSDBabySoC.git

Use make command to generate .V files:

    cd VSDBabySoC
    make pre_synth_sim

Work around for missing sand-piper on VM's:

![Screenshot from 2024-08-21 02-59-24](https://github.com/user-attachments/assets/7737c2e9-d854-4114-b6a7-0c4355b0b576)

    python3 -m venv .venv
    source ~/.venv/bin/activate
    
![Screenshot from 2024-08-21 03-08-45](https://github.com/user-attachments/assets/f5e95f45-14c9-4437-b39b-4e932f432394)

    python3 -m install sandpiper-saas    

Execute the command:

    sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
    
It will generate following .v files rvmyth.v and rvmyth_gen.v.


view the ouput vcd file:
    cd /home/nkm/Desktop/vsd-sfal/VSDBabySoC/output/pre_synth_sim/pre_synth_sim.vcd

waveform: DAC output is verified along with out

![Screenshot from 2024-08-21 02-45-06](https://github.com/user-attachments/assets/db30e618-563e-4b0b-a136-7a239c1ffaf0)

Data type used to get the outputs: D[9:0]: Decimal | out: Analog:step


</details>

<details>
<summary> Post GLS simulation </summary>

Post GLS simulation is performed by synthesizing netlist using dc_shell and verify the simulation using iverilog.

    pip3 install pyyaml click sandpiper-saas #install sandpiper to synthesize tlv files

SandPiper SaaS Edition runs Redwood EDA's SandPiper™ TL-Verilog compiler as a microservice in the cloud to support low-overhead and zero-cost open-source development using commercial-grade capabilities. This simple Python script provides a convenient command-line interface to the microservice. It is used by exciting projects such as WARP-V and 1st CLaaS.

A TL-Verilog-enhanced open-source Verilog development flow might also make use of the similarly-light-weight makerchip-app for TL-Verilog editing.

source: https://pypi.org/project/sandpiper-saas/

DC_Shell:

Dc shell needs propreitarry .DB file in place of .lib file to do mappping.
The git clone of https://github.com/manili/VSDBabySoC.git has .lib files of all the deisgn elements.

![image](https://github.com/user-attachments/assets/c844bee0-dd92-4502-bd05-332057df81fe)

we need to convert these .lib to .db using lc_shell: A Synopsys library compiler tool.

![image](https://github.com/user-attachments/assets/5d661e3e-fd06-48d3-b4e5-1a21596e7ac2)

commands used to convert: .lib to .db

fix all the errors per the log file: 

![image](https://github.com/user-attachments/assets/f694d4a0-b52c-47f8-8151-fd617bcd0f33)
   
after fixing the errors:

![image](https://github.com/user-attachments/assets/1d7f16e6-dcb4-4dd0-96b6-60096ac39bd1)

Note: Warnings can be ignored at this stage.

using wget command to import latest lib from git repo

![image](https://github.com/user-attachments/assets/a605a410-2eac-4fa5-9ca8-463b6b30dd8e)



commands used to convert:.lib to .db

    read_lib avsddac.lib
    write_lib avsddac.lib -format db -output avsddac.db
    write_lib avsddac.lib -format db -output avsddac.db
    read_lib avsddac.lib
    write_lib avsddac -format db -output avsddac.db
    read_lib avsdpll.lib
    write_lib avsdpll -format db -output avsdpll.db
    read_lib sky130_fd_sc_hd__tt_025C_1v80.lib
    write_lib sky130_fd_sc_hd__tt_025C_1v80 -format db -output sky130_fd_sc_hd__tt_025C_1v80.db


genearte neccesary .vh files: make pre_synth_sim

    sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/

using dc_shell to synthesize the netlist:

    set target_library /home/nanda/babysoc/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db
    set link_library {* /home/nanda/babysoc/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.db /home/nanda/babysoc/VSDBabySoC/src/lib/avsddac.db /home/nanda/babysoc/VSDBabySoC/src/lib/avsdpll.db }
    set search_path {/home/nanda/babysoc/VSDBabySoC/src/include /home/nanda/babysoc/VSDBabySoC/src/module/}
    read_file {sandpiper_gen.vh  sandpiper.vh  sp_default.vh  sp_verilog.vh clk_gate.v rvmyth.v rvmyth_gen.v vsdbabysoc.v} -autoread -top vsdbabysoc
    link
    compile_ultra
    write_file -format verilog -hierarchy -output /home/nanda/babysoc/VSDBabySoC/output/babysoc_netlist.v
    report_qor > qor.txt
    pwd
    write_file -format ddc -hierarchy -output /home/nanda/babysoc/VSDBabySoC/output/babysoc.ddc
    history > history.tcl


read the design collaterals

![image](https://github.com/user-attachments/assets/6e236fb6-0e37-4d6f-bfd4-8874ed28d4a1)

link the design:
    link

![image](https://github.com/user-attachments/assets/65511bce-1928-40bd-88c8-98e674c892b4)

    report_qor
    
![image](https://github.com/user-attachments/assets/3ed831d5-2d76-4f8f-8292-86b4a8d04076)

![image](https://github.com/user-attachments/assets/2e4b8bed-4195-444b-b18b-b4574f6fd06f)

using Iverilog to get post synth simulation vcd file.

    iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/babysoc_netlist1.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v

post synth sim:

![image](https://github.com/user-attachments/assets/2e8c1095-f7bc-4014-ac60-c8b642759435)


pre synth sim:

![Screenshot from 2024-08-21 02-45-06](https://github.com/user-attachments/assets/db30e618-563e-4b0b-a136-7a239c1ffaf0)


</details>

<details>
<summary>
Using PT to analyze the timing corners. </summary>
    
## PVT: Process Voltage Temperature##

### essential for simulating any IC for different weather conditions in order to abe able to work efficiently and be reliable.
IC's are tested from -40C to +125C using industry standard tools and fabricated with advance packaging solutions like quad package, 2.5D etc.

We are using SKY130PDK PVT LIBS in our design


(https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing)

TCL script used to convert libs to db

    
    # convert_lib_to_db.tcl
    set lib_files_dir "/home/nanda/babysoc/VSDBabySoC/src/lib/skywater-pdk-libs-sky130_fd_sc_hd/timing";
    set db_output_dir "/home/nanda/babysoc/VSDBabySoC/src/lib/timinglibs";
    foreach lib_file [glob -nocomplain $lib_files_dir/*.lib] {
    set base_name [file rootname [file tail $lib_file]]
    set db_file "$db_output_dir/${base_name}.db"

    if {[llength [list_libs]] > 0} {
        remove_lib [lindex [list_libs] 0]
    }

    read_lib $lib_file

    write_lib $base_name -format db -output $db_file

    if {[llength [list_libs]] > 0} {
        remove_lib [lindex [list_libs] 0]
    }
    }
    exit

<img width="1283" alt="Screenshot 2024-09-03 at 9 37 09 PM" src="https://github.com/user-attachments/assets/6ac22f5b-6b22-4890-8720-742ee896a68e">

open PT_SHELL

TCL script used in PT_SHELL

    set m1 ""
    set pvt ""
    set wns ""
    set whs ""
    set FH [open report_timing_prime_time.rpt w]
    puts $FH "PVT_Corner\tWNS\tWHS"
     set lib_files [glob -directory /home/nanda/babysoc/VSDBabySoC/src/lib/timinglibs/ -type f *.db]
    foreach lib_file_paths $lib_files {
	regexp {.*\/sky130_fd_sc_hd__(.*)\.db$} $lib_file_paths m1 pvt
    set link_path "* /home/nanda/babysoc/VSDBabySoC/src/lib/avsddac.db /home/nanda/babysoc/VSDBabySoC/src/lib/avsdpll.db "
    lappend link_path $lib_file_paths

    read_verilog "/home/nanda/babysoc/VSDBabySoC/output/babysoc_sdc_gnet.v"
    current_design vsdbabysoc

    link_design
    read_sdc "/home/nanda/babysoc/VSDBabySoC/babysoc.sdc"
    read_parasitics "/home/nanda/babysoc/VSDBabySoC/output/empty.spef"


    set wns [get_attribute [get_timing_paths -delay_type max -max_paths 1] slack]
    set whs [get_attribute [get_timing_paths -delay_type min -max_paths 1] slack]

    puts $FH "$pvt\t$wns\t$whs"

    remove_annotated_parasitics -all
    reset_design
    remove_design -all
    remove_lib -all
    }
    close $FH


Timing info for different corners

	PVT_Corner	WNS		WHS
	ff_100C_1v65	2.554515	-0.250917
	ff_100C_1v95	4.066801	-0.304045
	ff_n40C_1v56	0.778143	-0.208451
	ff_n40C_1v65	1.911201	-0.244908
	ff_n40C_1v76	2.930052	-0.275657
	ff_n40C_1v95	4.091851	-0.312528
	ss_100C_1v40   -18.62158	 0.405345
	ss_100C_1v60   -9.373999	 0.142039
	ss_n40C_1v28   -64.063217	 1.329605
	ss_n40C_1v35   -41.196972	 0.847522
	ss_n40C_1v40   -31.193745	 0.624912
	ss_n40C_1v44   -25.407854	 0.490901
	ss_n40C_1v60   -12.109005	 0.162826
	ss_n40C_1v76   -5.881881	 0.003838
	tt_025C_1v80    0.439206	-0.190414
	tt_100C_1v80	0.593517	-0.185542




Graph for WNS: Worst negative slack (Setup)

<img width="362" alt="image" src="https://github.com/user-attachments/assets/dafd39c2-9366-4910-b730-47860dc93117">

Graph for WHS: Worst hold slack(hold)

<img width="362" alt="image" src="https://github.com/user-attachments/assets/9a1a5069-88db-4156-a362-ca60592c9f29">

</details>

<details>
<summary> 
Physical design flow
</summary>
	
Collaterals needed to start Physical Design.

1. .Verilog: RTL netlist
   
3. .Lib : logical library
   
5. .SDC : Synopsys design constraints for constarining the design
   
7. .LEF/.DEF : abstract and detailed view of Cells/Macros/deisgn
   
9. .TF : Technology file usually from Foundry
    
11. .UPF : Unified power format aka low power congig file.


Perform Sanity checks on all these inputs file and start the PnR flow.

Steps involced in PnR:

****Synthesis** ----------> Floorplan ----------> Placement ----------> CTS ----------> Routing ---------> Sign Off.**

**Synthesis:**

Convert RTL to Synthesized netlist using tool commands and obtain gate level netlist w.r.t .lib used

verify reports like timing, Qor, WLM etc.

completeness of Synthesis N/L, Updated SDC, updated DFT, updated UPF

Snippets:

![image](https://github.com/user-attachments/assets/e8a3a7b1-fb6a-4c3d-9636-ec40efd76d09)

DFF covergae = 1613/18036= 8.94%



**Floorplan:**

Real Estate of the design.

Top to Bottom or vice versa approach.

shape creation

Macro placement

I/O placement

Power planning

Partitioning

BUS Planning, Repeater/FT planning.

Physical cell usage

Finalize 

Verify reports like Timing,QoR, Uril number, PG planning/routing

Snippets:

![image](https://github.com/user-attachments/assets/4d87bd71-ac93-4ab8-a5e6-b1f5bb590e7e)
![image](https://github.com/user-attachments/assets/a3281c91-9a0a-4f49-b830-7c740628cc78)
![image](https://github.com/user-attachments/assets/b07c6e7d-46ec-43b1-969b-047e827db779)





**Placement:**

Placment of Std cells in the designeated core area.

Coarse placment

Detailed placment

Place opt

Verify Reports: Timing, QoR, HFNS, Scan Congig, Util number, Legality, Congestion etc

Snippets:

![image](https://github.com/user-attachments/assets/f7d5b3db-3831-491f-82be-bf9234e8a0e5)
![image](https://github.com/user-attachments/assets/81775439-05ab-4a63-8d44-ea30ea135b9f)


**CTS:**

Building good CTS.

good CTS plannning like CTmesh, H-Tree, X-tree, Binary.

Good LOL for Clock drop off points in the design.

Source, Network Latency

Skew and Insertion delay.

Usage of Clock buffers and Inverters.

CLK routing and its impact on placed std cells.

Verify Reports: QoR, Timing, CT numbers, Util number etc




**Routing:**

Global Routing

Detailed Routing

Search and Repair

Grid based Algorithms

Routable design

Verify Reports: Qor, LVS, DRC, Timing etc

**Sign Off:**

This is a very effective and strong step which can be used at any step in PnR to make sure to have a solid IC.

Formal Verification

Physical Verification

CLP: Conformal Low Power

IR analysis

STA: Static Timing Analysis


	
PnR flow using open source Openlane flow, available for free of cost and can be used to get a GDS file, which can be fabricated using SKY130PDK or custome PDK's.

**PnR flow:**

![image](https://github.com/user-attachments/assets/99c5ae41-527a-4a15-b60d-51d32bd8aac2)
source: Google


**Openlane Infrastructure:**

**RTL to Netlist**

: Linting / Verilator

: Power Distribution Network Hierarchy / Yosys

: Synthesis / Yosys

: Synthesis / Design Compiler (with proprietary plugin)

: Multi-corner Netlist STA / OpenSTA

**Floorplanning**

: Floorplan Initialization / OpenROAD

: Manual Macro Placement / OpenDB

: Tap/Endcap Insertion / OpenROAD

: PDN Generation / OpenROAD

**Placement**

: Pin Placement (from config file) / OpenROAD, OpenDB

: Pin Placement (Random/Matching/Annealing) / OpenROAD

: Pin Placement (from template DEF) / OpenDB

: Global Placement / OpenROAD

: Resizer Design Repair (Post-GPL) / OpenROAD

: Detailed Placement / OpenROAD

**Clock Tree Synthesis**

: Clock-Tree Synthesis / OpenROAD

: Resizer Timing Repair (Post-CTS) / OpenROAD

**Routing**

: Global Routing / OpenROAD

: Resizer Design Repair (Post-GRT) / OpenROAD

: Diode Insertion on Ports / OpenDB

: Heuristic Diode Insertion / OpenDB

: Antenna Repair / OpenROAD

: Resizer Timing Repair (Post-GRT) / OpenROAD

: Detailed Routing / OpenROAD

: Row Filling / OpenROAD

**Signoff (Timing)**

: Parasitics Extraction / OpenROAD

: Multi-corner Static Timing Analysis / OpenSTA

: SI-Enabled Multi-corner Static Timing Analysis / PrimeTime (with proprietary plugin)

**Signoff (Physical)**

: GDSII Stream-Out / Magic

: GDSII Stream-Out / KLayout

: Magic vs. KLayout Stream XOR / KLayout

: Design Rule Checks / Magic

: Design Rule Checks / KLayout

: Spice Extraction / Magic

: Layout vs. Schematic / Netgen

: Equivalence Check (Alpha) / Yosys EQY

Source:	https://github.com/efabless/openlane2.git


In our test case we are using Picorv32a design:

The PicoRV32A design in OpenLane is an implementation of the PicoRV32, a minimalistic and highly configurable open-source RISC-V CPU core. It is a small, compact, 32-bit CPU that implements the RISC-V instruction set architecture (ISA).

</details>

<details>
<summary>Standard Cell Characterization:</summary>
	
![image](https://github.com/user-attachments/assets/025b0c99-7dc1-41c4-a7e4-722a4009b805)
![image](https://github.com/user-attachments/assets/b6a61163-cb9f-4efa-b431-ef7c2f442b59)

source: google

	
All Standard cells are characterized using SPICE models. These standard cells are implemented using NAND or NOR gates, which are called universal gates. The cell desnsity and node technology gives a hint on which gates can be used to implement the functionality.

M1 and M2 layers are used for STD cells routing and the rest layers can be used for CLK, SIGNAL, POWER Routing.

These STD cells need to sit in UNIT SITE, that is the minimum cell size between standard cell rows and any cell of any flavour need to be in multiples of minimum unit site.

**Inputs:**

PDK

DRC and LVS Deck

Spice models

library and user defined specs: Metaly layers used, Operating voltage for the IC/Design, Pin Location.

**Design Steps:**

Circuit Design: CMOS gate level implememtation and W/L ratio of pmos and nmos.

Layout Design: Layout for the Circuit implemeted. The Euler's path and stick diagram

Characterization: 

Use RC extraction from the layouts and genearate Timing, Noise, Power .libs function


Outputs:
CDL: Circuit Description language.
GDS2, LEF, Extracted Spice netlist.

![image](https://github.com/user-attachments/assets/cd008980-3ffe-4e64-9bff-4d41d6a65a0e)
![image](https://github.com/user-attachments/assets/95cfd816-e6b2-41da-b438-c9bb65571d32)


Characterization Flow:

1.Read Model File from Foundry

2.Read Extracted spice netlist

3.understand Logic under test

4.Read subcircuit of DUT

5.Attach power sources

6.Apply stimulus

7.Apply caps based on NLDM and CCS

8. Neccesary simulation commands based on .DC and .trans simulation

9. Feed 1 thru 8 to "GUNA": Characterization software.

10. GUNA genereates: Timing, Power and Noise characterization.

![image](https://github.com/user-attachments/assets/936cf568-9faf-46fd-847c-6ab4da95411a)


**Spice Deck:**

![image](https://github.com/user-attachments/assets/820efdb7-5b37-4555-afa0-2903dcb00813)
![image](https://github.com/user-attachments/assets/f0e62d6e-a132-414d-9eeb-b359679c891e)
![image](https://github.com/user-attachments/assets/e6cf1278-9c1e-419c-a258-d6f5a3b66e1f)
![image](https://github.com/user-attachments/assets/141cd660-0eda-40f3-8065-49dfcc76233c)

# Sky130INV characterization

Inverter layout using MAGIC:

![image](https://github.com/user-attachments/assets/7239b7c8-326f-4d33-8c68-74fd9897d5c7)

using MAGIC to generate spice cir to analyse slew, Rise and Fall delays.
	extract all: command used to get ext file
 	ext2spice cthresh 0 rthresh 0: from ext to spice for RC parasitics
  	ext2spice: to get spice model which can be used to run in ngspice

INV spice model snippet:
![image](https://github.com/user-attachments/assets/6f9e2888-dac6-4ee0-bde5-1f72fc85f191)

ngspice commands:
	ngspice sky130_inv.spice
 	plot y vs time a

![image](https://github.com/user-attachments/assets/31eaa00e-153a-46e4-99aa-b72f0a9f9ce3)

![image](https://github.com/user-attachments/assets/b44ed917-6acf-474b-8c2d-794594f8baf0)





Snippet from Waveform for rise and fall transition:

![image](https://github.com/user-attachments/assets/2e0462cb-ea02-4979-a24c-c0037dd3f5ba)

Input rise trainsition: 80% of input - 20% of input
: 2.24598 - 2.1821
: 0.06388ns = 63.8ps
	    

Output fall Trainsition: 20% of output - 80% of output
:4.09529 - 4.0527
: 0.04253 ns = 42.53ps


Snippet for Rise and Fall delay:

![image](https://github.com/user-attachments/assets/d469a8c2-0a82-467b-92bb-d6f8e3340b57)

Cell Rise Delay: 50% of output rise - 50% of input fall
: 2.211107 - 2.15005 = 0.061057ns = 61.05ps

Cell Fall Delay: 50% of Output fall - 50% of input rise
: 4.07771 - 4.05003 = 0.02768ns = 27.68ps


# Fixing DRC issues
	wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 
	tar xfz drc_tests.tgz
	cd drc_tests
	magic -d XR 

 Doc to refer: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#x

 Example 1: poly.mag

 ![image](https://github.com/user-attachments/assets/91f4afeb-fc8f-47c5-9907-9ba9715d5310)

 ![image](https://github.com/user-attachments/assets/8932bb14-a9c0-4f90-999b-f57ca5ad63f9)

 editing tech file, manually adding poly rules for poly.9
 
 ![image](https://github.com/user-attachments/assets/d58401e6-8497-409a-9f7b-a55a2eb4df04)


Once tech file is modified, no need to start a new MAGIC session.

Steps to follow:

	tech load sky130A.tech
 	drc check # refreshes the design again
  	; select area
   	drc why


# using custom inv cell in design:

![image](https://github.com/user-attachments/assets/dc68cb21-ddd2-410d-9d08-9b042cb69da0)

3 essential checks to verify the layout is fine before writing out lef.

1. I/O port must align at the intersection of H and V tracks
2. WIDTH of the designed cell should be in odd multiples of the grid.
3. HEIGHT of the designed cell should be odd multiples of the grid.
4. The reason being to accomodate routing tracks efficiently.

The grid is aligned based on the track info of the design.
In our case:
grid 0.46um 0.34um 0.23um 0.17um

![image](https://github.com/user-attachments/assets/cc3a2e7a-fec2-4b61-856c-0570b5c4f151)

Steps before writing LEF:

	goto A
 
 	select area
  
  	what
   
   	port class input
    
	port use signal
   	
   
click on "edit"

![image](https://github.com/user-attachments/assets/5839f6a9-662d-478a-a0b5-26e852331c17)


repeat this for Y, VGND, VPWR

![image](https://github.com/user-attachments/assets/f3166660-99f9-404c-a914-8b653d62baa1)

write lef:
	lef sky130_vsdinv_nanda.lef

![image](https://github.com/user-attachments/assets/36cc90fe-b176-4447-8c0f-07ad4dacdc1a)

# Running OpenLane to source lef extracted from the inv.

Commands used:
	docker
 	./flow.tcl -interactive
	package require openlane 
	prep -design picorv32a
	set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]


 Params to be added in picorv32a/config.tcl
	#set the following in config.tcl
	set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
	set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
	set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
	set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
	set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

 1. run_syntheis:
    	with default params:



  	






 


 
      






</details>










































   










































     
