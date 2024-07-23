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
yosys
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
     used to verify the rtl code for basic gates, adders, multipliers, adders etc. 
     All the verilog codes have assosciated Test bench. 
     Iverilog uses two arguments to dump a intermediate a.out file, which can be run in terminal to get the the VCD file.
     VCD: Value change dump
     
![Screenshot from 2024-07-23 03-01-24](https://github.com/user-attachments/assets/060e9b70-54c1-46b8-95a4-5f03f40a3157)


vcd file is launched using gtkwave to verify the waveform
     ![Screenshot from 2024-07-23 23-46-41](https://github.com/user-attachments/assets/20fd89c5-7d97-451e-a639-ca7bff8639d9)
     ![Screenshot from 2024-07-23 23-47-33](https://github.com/user-attachments/assets/294f982d-200d-4c10-93a4-18c72ba35c47)
Snippet from Test Bench to verify the output of counter:
     ![Screenshot from 2024-07-23 23-51-20](https://github.com/user-attachments/assets/bb801a3e-9d40-4f04-bcce-cec7e3b95482)









     
