# vsd-sfal
Day 1: Install yosys, iverilog and gtkwave
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


show: before opt_purge -clean

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
This can break the circuit functionality.

verilog snippet:
![Screenshot from 2024-07-27 23-54-32](https://github.com/user-attachments/assets/443e3290-9f78-4f3e-866b-25450ebb6810)


gtkwave simualtion:
![Screenshot from 2024-07-27 23-53-29](https://github.com/user-attachments/assets/7fd8d2b4-357c-42f3-b685-6c6cbe3dc3cb)


show:
![Screenshot from 2024-07-27 23-49-57](https://github.com/user-attachments/assets/0ad89813-ffbe-4a33-9e1b-a2db413d3f53)


gate level netlist:
![Screenshot from 2024-07-27 23-51-24](https://github.com/user-attachments/assets/3fa31271-e753-46b9-859c-bf029b608dfd)










































     
