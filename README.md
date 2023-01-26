# miit_fdp

# Day1 Content

# Introduction to Verilog RTL design and Synthesis

# Labs using iverilog and gtkwave

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

# Labs using Yosys and Sky130 PDKs

Commands to obtain a synthesized implementation of good_mux design

The commands to synthesise a rtl code called good_mux is as follows:

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

![image](https://user-images.githubusercontent.com/123365828/214258249-c63ab906-0612-4733-89cd-22890799f21d.png)

![image](https://user-images.githubusercontent.com/123365828/214258333-0f74d627-de0a-40fe-a92f-9e58d5969e4d.png)

![image](https://user-images.githubusercontent.com/123365828/214258403-452f8085-0a7f-41cb-8ef4-a0160c6e01f9.png)

Commands to write a netlist

write_verilog -noattr good_mux_netlist.v

!vim  good mux_netlist.v

![image](https://user-images.githubusercontent.com/123365828/214258553-689257e7-3c06-404e-9c3f-97f83e88e008.png)

# DAY2 : TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES

!vim ../lib/SKY130_fd_sc_hd__tt_025C_1v80.lib

The following window appears that shows the library file SKY130_fd_sc_hd__tt_025C_1v80.lib

![Capture1](https://user-images.githubusercontent.com/123365828/214277544-844e968f-a181-48e5-8fdf-7cefe8c26809.PNG)

Switching off the syntax color red

  Command used : syn off
  
Enabling the line numbers

  Command used :se nu

We can see that the lib file contains the technology and units of all the parameters.

![Capture2](https://user-images.githubusercontent.com/123365828/214278585-be2b6a7f-ac71-46b7-a076-6050117d8b69.PNG)

The voltage process and temperature conditions are also specified.

![Capture3](https://user-images.githubusercontent.com/123365828/214279285-f32862c7-e24f-4a0e-af22-8cb253921fa5.PNG)

The lib contains different flavors of this same as well as different types of cells.

![Capture4](https://user-images.githubusercontent.com/123365828/214279642-0cc0864f-2315-4d0b-a836-c025d3baec4f.PNG)

![Capture5](https://user-images.githubusercontent.com/123365828/214280120-a2ed7c69-6a8b-4518-a504-e607a70bb87b.PNG)

As we see in the above window,

The library also represents the different features of the cell like its leakage power,the various input's combinations and the operations between them.

 Inside the cell block we have different power combinations and their respective leakage power values. It also shows the area. It gives the description of various pin in terms of their capacitance transition , internal power and the delay associated with this pins.
 
 ![Capture6](https://user-images.githubusercontent.com/123365828/214297435-9630a58b-4e40-4b77-b883-3d37c296dd7f.PNG)
 
 We pick a small gate small gate for better understanding. We see it's behaviour view.
 
 ![Capture7](https://user-images.githubusercontent.com/123365828/214298354-fbcc5ad5-e14c-4d23-89a4-f6bd62b45f9e.PNG)
 
 We now perform the comparison between the and gates.
 
 ![Capture8](https://user-images.githubusercontent.com/123365828/214306598-1fbaf219-43b4-4f04-98f3-37c021c60e0f.PNG)

# HIERARCHIAL VS FLAT SYNTHESIS

While syntheisizing the RTL design in which multiple modules are present, the synthesis can be done in two forms.

vim multiple_modules.v

![Capture9](https://user-images.githubusercontent.com/123365828/214307541-0ffa8ce6-c2b9-452e-ae83-8c2f56c30c61.PNG)

It has two some moduels. The module 1 is an OR gate ,sub module 2 is AND gate. The sub module called multiple modules instantiates sub module 1 as u1 and sub module 2 as u2.

yosys

![Capture10](https://user-images.githubusercontent.com/123365828/214310390-2b79ad42-d866-41b6-b8b9-446933b8fab2.PNG)

![Capture11](https://user-images.githubusercontent.com/123365828/214311042-47416020-e2eb-467d-a5a0-6552643d48b3.PNG)

![Capture12](https://user-images.githubusercontent.com/123365828/214311323-847f4ac3-aede-438b-8120-4a3564c41b05.PNG)

Now we link this design to the library using abc command. To show the graphical version ,we use the command.

show multiple_modules

![Capture13](https://user-images.githubusercontent.com/123365828/214311722-ed198835-8a18-4057-a870-5f9164c2919b.PNG)

Instead of or and and gates it shows the instances u1 and u2 while preserving the hierarchy. This is called the hierarchical design.

We use flatten to generate a flat netlist. Here there are no instances of U1 and U2 and hierarchy is not present.

![Capture14](https://user-images.githubusercontent.com/123365828/214313683-c6a17a25-2ca8-4efc-a075-0d81def0d83e.PNG)

‌‌Even in the design view using show command we see that it simply displays the structure completely without any hierarchy.

![Capture15](https://user-images.githubusercontent.com/123365828/214318050-75229e09-433e-4ebe-b348-321add3962ef.PNG)

# SUB MODULE LEVEL SYNTHESIS AND ITS NECESSITY

Hence we control the model that we are synthesizing using the keywords

  synth-top "sub-module's name"
  
  In the following example, I am going to synthesize at sub_module 1 level although I have read the RTL file at higher module level multi_modules.v

read_verilog multiple_modules.v

synth -top sub_module1

![Capture16](https://user-images.githubusercontent.com/123365828/214321876-8d972232-787c-4615-9abb-067b874ca286.PNG)

Notice ,In the synthesis report,it inferring only 1 AND gate.

# Asynchoronous and Synchronous resets

Verilog codes of asynchronous reset and set :

![Capture17](https://user-images.githubusercontent.com/123365828/214333940-519a92cf-a80f-4c77-aa5e-c61b2d02ed7f.PNG)

GTKWAVE RTL Simulation and Observations :

![Capture18](https://user-images.githubusercontent.com/123365828/214339590-30e8c402-9fbf-41b7-a0af-62bb26a1a3cf.PNG)

Q follows d only at the posedge of the clock.
But as and when async_rest=1,Q becomes 0 without waiting for the next edge of the clock.

![Capture19](https://user-images.githubusercontent.com/123365828/214340049-a8344f5f-e529-4d78-970e-8ccebe25303c.PNG)

But when async_reset goes low(1 to 0),Q doesn't become 1 immediately ,it waits for the next clock edge to follow D.
Even if asunc_reset=1 and D=1, Q=0 as reset takes high precedence(that is how the code has been written,if condition of reset is checked first).

Synthesis implementation results :

asynchoronous reset :

![Capture20](https://user-images.githubusercontent.com/123365828/214342753-e7234b7d-1b1f-4356-86a6-6df13eebe339.PNG)

asynchronous set:

![Capture21](https://user-images.githubusercontent.com/123365828/214500662-019663b7-a1a3-4c00-a080-ff4fa4b15c73.PNG)


Synchronous Reset and set : On application of set or reset signals,the signal does not immediately goes high or low,but it waits for the next edge of the clock cycle. RTL code of synchronous reset:

![Capture22](https://user-images.githubusercontent.com/123365828/214346188-48aa20a7-bc72-4cf6-a2c3-67d3b8828203.PNG)

Synthesis Results:

![Capture29](https://user-images.githubusercontent.com/123365828/214496454-c3a8e8ec-1b1b-406f-b7ca-1020f5bbb214.PNG)

Case wherein both both asynchronous and synchronous resets are applied together :

RTL CODE:

![Capture24](https://user-images.githubusercontent.com/123365828/214348058-d52e669d-661f-4e58-bdef-a0b32b4b7363.PNG)

Synthesis results:

![Capture25](https://user-images.githubusercontent.com/123365828/214500040-ace712d4-73c4-4ab0-b8c4-bce2c4d596ce.PNG)


# OPTIMISATIONS

We now observe some Interesting Optimisations For Special Cases :

Case 1:

Let's Consider the following design where the 3 bit input is multiplied by 2 and the output is a 4 bit value.

module mul2 (input [2:0] a , output [3:0] y);

	assign y = a* 2;
 
endmodule

Observation : The output y[3:0] is the input a[2:0] appended with a 0 at the LSB. or, we can say that y = aX2 = {a,0} .

On synthesizing the netlist and look at its graphical realisation , we will see the same optimisation occuring in the netlist.There is no hardware required fot it.

![Capture26](https://user-images.githubusercontent.com/123365828/214351670-089578a9-0a0e-41f1-ae18-994c9f56c1c9.PNG)

Case 2:

Let's consider the following design where the 3 bit input is multiplied by 9 and the output is a 6 bit value.

module mult8 (input [2:0] a , output [5:0] y);

	assign y = a* 9;
 
endmodule

Observation : The output y[5:0] is equal to the input a[2:0] appended with itself i.e.

y = aX9 = aX(8+1)= aX8+aX1 = {a,0}+a 

y = {a,a}

On synthesizing the netlist and look at it's graphical realisation , we will see the same optimisation occuring in the netlist.

![Capture27](https://user-images.githubusercontent.com/123365828/214353493-7b941831-3a58-4ef1-b722-d942e9f0c7ec.PNG)

# DAY 3 : Combinational and Sequential Optimisations

# Introduction to Logic optimisations

The simulator performs multiple types of optimization techniques on the combinational and sequential circuits in order to provide a digital circuit design that is optimized in terms of area and power.

1.Combinational optimisation methods:

-Squezzing the logic to get the most optimised design

-Area and Power savings

-Constant propogation

-Direct Optimisation

-Boolean Logic Optimisation

-K-map

-Quine-mckluskey Algorithm

2.Sequential optimisation methods:

-Basic

-Sequential constant Propogation

-Advanced

-State Optimisation

-Retiming

-Sequential Logic Cloning (Floor Plan Aware Synthesis)

# Combinational Logic Optimisations

To understand each of the above mentioned combinational optimizations through different RTL code examples, also check the synthesis implementation through yosys to understand how the optimisations take place.

All the optimisation examples are in files opt_check.v, opt_check4.v, and multiple_modules_opt.v. All of these files are present under the verilog_files directory.

Example 1:

opt_check.v

module opt_check (input a, input b , output y);

	assign y = a?b:0;
	
endmodule

The ternary operator mentioned above should produce a mux. The reasoning, however, continues to propagate the constant 0. We get y = ab by using boolean simplification.

Synthesizing this in yosys :

We need to provide Yosys a command to optimize things before realizing the netlist. To create an optimized digital circuit, all unused cells and wires are removed. The opt clean -purge command can be used to accomplish this, as seen below.

![Capture1](https://user-images.githubusercontent.com/123365828/214542383-fd51671c-237e-4099-8d45-f867f8f6e286.PNG)

Observation: When we run synth -top opt_check, we observe that 1 AND gate has been inferred in the report.

Next,

 abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 
 
 write_verilog -noattr opt_check_netlist.v 
 
 show

We can see that the Yosys has synthesized an AND gate as anticipated by checking at the graphical synthesis realisation.

![Capture2](https://user-images.githubusercontent.com/123365828/214544597-3675e826-cd7d-427e-9c50-3c0d10d34d67.PNG)

Example 2:

opt_check2.v

module opt_check2 (input a, input b , output y);

	assign y = a?1:b;
	
endmodule

Since the output of the mux can be reduced to y = a + b, we anticipate that the output y will be an OR gate after simplification. If we create the netlist and examine it in graphical form, we get

![Capture3](https://user-images.githubusercontent.com/123365828/214546689-b37b6518-b6d6-4537-aa54-feff3d8efff9.PNG)

Note: Based on Demorgan's Law, the synthesis tool infers a nand gate with inverted inputs in place of OR gates. To prevent stacking PMOS in CMOS implementation of the OR gate, this is done.

Example 3: opt_check3.v

module opt_check3 (input a, input b , input c , output y);

	assign y = a?(c?b:0):0;
	
endmodule

We expect that the output of the RTL verilog code for opt check3.v will be a 3 input AND gate based on constant propagation and boolean logic optimization. It is possible to reduce the output y to y = abc.

Then, we generate the netlist and study at its graphical form after synthesis

![Capture4](https://user-images.githubusercontent.com/123365828/214550431-6c0055d9-d948-4788-bf28-d181cdba7c05.PNG)

Yosys synthesizes a 3 input AND gate as expected because of optimisations.

Example 4:opt_check4.v

module opt_check4 (input a, input b , input c , output y);

	assign y = a?(b?(a & c):c):(!c);
	
endmodule

Boolean logic optimization reduces the output in this instance to a single xnor gate, y = a xnor c. Following that, we generate the netlist and study at its graphical depiction after synthesis

![Capture5](https://user-images.githubusercontent.com/123365828/214553656-d7491278-4c60-4661-8215-c5669cbeb467.PNG)

Yosys synthesizes a 3 input XNOR gate as expected because of optimisations.

Example 5:multiple_module_opt.v

module sub_module1(input a , input b , output y);

	assign y = a & b;
	
endmodule

module sub_module2(input a , input b , output y);

	assign y = a^b;
	
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);

wire n1, n2, n3;


sub_module1 U1 (.a(a), .b(1'b1), .y(n1));

sub_module2 U2 (.a(n1), .b(1'b0), .y(n2));

sub_module2 U3 (.a(b),  .b(d), .y(n3));

assign y =c | (b & n1);

endmodule

![Capture6](https://user-images.githubusercontent.com/123365828/214555292-c8041a6e-d824-47e2-9188-84c0a94f0e38.PNG)

In yosys, we use flatten before opt clean -purge when synthesizing this. Both submodules 1 and 2 are instantiated by the multiple module opt. Here, Flat Synthesis is required; else, the sub module level optimizations won't be carried out.

![Capture7](https://user-images.githubusercontent.com/123365828/214555728-2b923284-6e3b-4c41-a54d-83f36d011d1a.PNG)

Example 5: multiple_module_opt2.v

![Capture8](https://user-images.githubusercontent.com/123365828/214556274-7be8a009-fccf-437d-ad49-6ac8d8bff55c.PNG)

On boolean optimisation, we obtain y=1 simply. It's synthesis yields:

![Capture9](https://user-images.githubusercontent.com/123365828/214556931-fdeb4e66-00a9-4cfb-a60c-3ee683c84d9e.PNG)

# Sequential Logic Optimisations

We will use many RTL code examples to try to understand each sequential optimization. To further understand how the optimizations are carried out, we additionally check the synthesis implementation for each example using yosys.

All the optimisation examples are in files dff_const2.v,dff_const3.v,dffconst4.v and dff_const5.v. All of these files are under the verilog_files directory.

Example 1: dff_const1.v

module dff_const1(input clk, input reset, output reg q);

always @(posedge clk, posedge reset)

begin

	if(reset)
	
		q <= 1'b0;
		
	else
	
		q <= 1'b1;
		
end

endmodule

In this case, the output Q should equal a reset that has been inverted, or Q=!reset. However, because the reset is synchronous, even if the flop has D pinned to logic 1, when reset turns into 0, Q does not go instantly to 1. It waits until the next clock cycle's positive edge.

This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows

![Capture10](https://user-images.githubusercontent.com/123365828/214559092-11ca01c3-7de2-4286-9119-1d297db20fd8.PNG)

Observation: When reset reaches 0, Q becomes 1 at the next clock edge in the gtk waveform above. We do not obtain a sequential constant because Q can be either 1 or 0, hence no optimizations ought to be achievable. Using Yosys synthesis and optimization, we verify it.

While synthesis,We use

dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

dfflibmap is a switch that tells the synthesizer about the library to pick sequential circuits( mainly Dff's and latches) from.

We then generate the netlist

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr dff_const1_netlist.v 

show

![Capture11](https://user-images.githubusercontent.com/123365828/214562722-23147829-d9b8-49a7-85fc-e04f4874d6fc.PNG)

As expected, No optimisation is performed in th yosys implementation during synthesis.

Example 2 : dff_const2.v

module dff_const2(input clk, input reset, output reg q);

always @(posedge clk, posedge reset)

begin

	if(reset)
	
		q <= 1'b1;
		
	else
	
		q <= 1'b1;
		
end

endmodule

Here, Regardless of the inputs, the output q always remains constant at 1 .

This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows

![Capture12](https://user-images.githubusercontent.com/123365828/214564430-4d9c030c-f35f-4d54-9f92-c54166fe9b0c.PNG)

Since the output is always constant i.e Q=1, it can easily be optimised during synthesis.

![Capture13](https://user-images.githubusercontent.com/123365828/214565465-fb3bb89c-9ef8-492f-9d22-a92ca7c7243f.PNG)

Example 3 : dff_const3.v

module dff_const3(input clk, input reset, output reg q);

reg q1;

always @(posedge clk, posedge reset)

begin

	if(reset)
	
	begin
	
		q <= 1'b1;
		
		q1 <= 1'b0;
		
	end
	
	else
	
	begin 
	
		q1 <= 1'b1;
		
		q <= q1;
		
	end
	
end

endmodule

In an ideal ckt, Q1 follows D at the next positive clock edge when reset changes from 1 to 0. But in reality, Q1 becomes 1 a little after the next positive clk edge(once reset has been made 0)due to Clock-to-Q delay.

Thus, q takes the value 0 until the next clock edge when it read an input of 1 from q1. This is confirmed with the simulated waveform below.

![Capture14](https://user-images.githubusercontent.com/123365828/214566952-b738a5b3-c3bc-4bf1-9f3f-8c771198b9fb.PNG)

Since Q takes both logic 0 and 1 values in different clock cycles. It is wrong to say that Q=!(reset) or Q=Q1

Hence, both the flip-flops are retained and no optimisations are performed on this design. We can confirm this using Yosys as shown below.

![Capture15](https://user-images.githubusercontent.com/123365828/214567999-c97f1a06-b0ca-46f6-bf76-61b6d1d1d90f.PNG)

Both the D flip-flops are present in the synthesized netlist.

Example 4: dff_const4.v

RTL Code:

![Capture16](https://user-images.githubusercontent.com/123365828/214568539-1bb25c1f-3e57-41e0-b5e5-020af438ab84.PNG)

Here, regardless of the input whether reset or not , Q1 is always going to be constant i.e. Q1=1 . As q can only be 1 or q1 depending on the reset input , but q1 = 1 .Thus q is also constant at the value 1. We can confirm this with the simulated waveforms as shown below.

![Capture17](https://user-images.githubusercontent.com/123365828/214569832-65bc23b1-1d61-4205-a4c6-a226ba37f12a.PNG)

As the output is always constant, it can easily be optimised using Yosys as shown in the graphical representation.

![Capture18](https://user-images.githubusercontent.com/123365828/214571342-d60b6791-c4ae-4dc7-acf5-e1496b954788.PNG)

unutilized output optimizations










