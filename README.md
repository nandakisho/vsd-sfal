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



















     
