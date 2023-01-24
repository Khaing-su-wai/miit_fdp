# miit_fdp
Day1 Content
Day 1 - Introduction to Verilog RTL design and Synthesis
Labs using iverilog and gtkwave
mkdir vsd  
 cd vsd  
 git clone https://github.com/kunalg123/vsdflow.git  
 mkdir vlsi  
 cd vlsi  
 git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git  
 cd SKY130RTLDesignAndSynthesisWorkshop  
 cd my_lib  
 cd lib  
 cd ..  
 cd verilog_model  
 cd ..  
 cd ..  
 cd verilog_files  
 Below screenshot shows the above directory structure inside the vsd upto my_lib directories that was set up through the terminal.
 
 ![image](https://user-images.githubusercontent.com/123365828/214255272-44106959-5909-47ff-bf6e-2610e6dcbac7.png)
 
![image](https://user-images.githubusercontent.com/123365828/214255695-edbf73db-6810-4915-b545-4a2ec5420e7f.png)

![image](https://user-images.githubusercontent.com/123365828/214255746-2d9ff244-d1d6-4f5d-a4ab-98469fb573e2.png)

![image](https://user-images.githubusercontent.com/123365828/214255774-45b44fc5-2ac0-4f56-8b33-48518e4701f4.png)

iverilog good_mux.v tb_good_mux.v

As a result of the above , a. file is created which can be seen in the list of verilog files.

![image](https://user-images.githubusercontent.com/123365828/214256470-0c2ee789-96b9-4724-8481-3e66e7575f96.png)

./a.out

gtkwave tb_good_mux.vcd

![image](https://user-images.githubusercontent.com/123365828/214256952-a6476c1d-a80e-4d3f-8bae-34dc723d6d0a.png)

![image](https://user-images.githubusercontent.com/123365828/214256981-d6e078fe-5fc7-404f-bcd5-22c81fbb1a2f.png)

We can also view our Verilog RTL design and testbench code using

vim tb_good_mux.v -o good_mux.v

![image](https://user-images.githubusercontent.com/123365828/214257254-72ddbb63-099a-49b4-941f-9a1d9398ba02.png)

![image](https://user-images.githubusercontent.com/123365828/214257295-be8ae55b-2a01-4a4e-9558-2b25599b8fc7.png)

Labs using Yosys and Sky130 PDKs

Commands to obtain a synthesized implementation of good_mux design

The commands to synthesise a rtl code called good_mux is as follows:

yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

![image](https://user-images.githubusercontent.com/123365828/214258249-c63ab906-0612-4733-89cd-22890799f21d.png)

![image](https://user-images.githubusercontent.com/123365828/214258333-0f74d627-de0a-40fe-a92f-9e58d5969e4d.png)

![image](https://user-images.githubusercontent.com/123365828/214258403-452f8085-0a7f-41cb-8ef4-a0160c6e01f9.png)

Commands to write a netlist

write_verilog -noattr good_mux_netlist.v

!vim  good mux_netlist.v

![image](https://user-images.githubusercontent.com/123365828/214258553-689257e7-3c06-404e-9c3f-97f83e88e008.png)

DAY2 : TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES


