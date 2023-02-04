# miit_fdp

# Day1 Content

## Introduction to Verilog RTL design and Synthesis

### Labs using iverilog and gtkwave

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

### Labs using Yosys and Sky130 PDKs

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

### HIERARCHIAL VS FLAT SYNTHESIS

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

### SUB MODULE LEVEL SYNTHESIS AND ITS NECESSITY

Hence we control the model that we are synthesizing using the keywords

  synth-top "sub-module's name"
  
  In the following example, I am going to synthesize at sub_module 1 level although I have read the RTL file at higher module level multi_modules.v

read_verilog multiple_modules.v

synth -top sub_module1

![Capture16](https://user-images.githubusercontent.com/123365828/214321876-8d972232-787c-4615-9abb-067b874ca286.PNG)

Notice ,In the synthesis report,it inferring only 1 AND gate.

### Asynchoronous and Synchronous resets

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


### OPTIMISATIONS

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

## Introduction to Logic optimisations

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

### Combinational Logic Optimisations

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

### Sequential Logic Optimisations

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

As a result, both flip-flops are retained, and this design has no optimizations. Using Yosys, we can verify this as illustrated below.

![Capture15](https://user-images.githubusercontent.com/123365828/214567999-c97f1a06-b0ca-46f6-bf76-61b6d1d1d90f.PNG)

Both the D flip-flops are present in the synthesized netlist.

Example 4: dff_const4.v

RTL Code:

![Capture16](https://user-images.githubusercontent.com/123365828/214568539-1bb25c1f-3e57-41e0-b5e5-020af438ab84.PNG)

Here, regardless of the input whether reset or not , Q1 is always going to be constant i.e. Q1=1 . As q can only be 1 or q1 depending on the reset input , but q1 = 1 .Thus q is also constant at the value 1. We can confirm this with the simulated waveforms as shown below.

![Capture17](https://user-images.githubusercontent.com/123365828/214569832-65bc23b1-1d61-4205-a4c6-a226ba37f12a.PNG)

As the output is always constant, it can easily be optimised using Yosys as shown in the graphical representation.

![Capture18](https://user-images.githubusercontent.com/123365828/214571342-d60b6791-c4ae-4dc7-acf5-e1496b954788.PNG)

unused output optimizations

# Day 4: Gate Level Simulations,Blocking vs Non Blocking assignments,Synthesis-Simulation Mismatch

## Introduction to Gate Level Simulations

We apply netlist to the test bench as desh under test while using GLS. In terms of the standard cells provided in the library, what we accomplished at the behavioral level in the RTL code was translated to the net list. Net list and RTL code are therefore equivalent logically. Since their inputs and outputs are identical, the netlist should fit exactly where the RTL code should have been. We replace the RTL file with the netlist and launch the simulation using the test bench.

The hold and setup times, which are crucial for a circuit, are not included in simulations carried out with the aid of RTL code. There are other cell flavors in the library that satisfy this setup and hold time requirement.

![image](https://user-images.githubusercontent.com/123365828/215011051-6f614ad6-0ee2-43f6-a2be-ea7178f319be.png)

When utilizing GLS with Iverilog Flow, the design is a netlist that is provided to the Iverilog simulator in terms of the library's standard cells. The same type of cell is accessible in the library in a variety of flavors. The GATE level verilog models are also provided as input to enable the simulator to comprehend the specification of the various annotations of the cell. We can utilize the GLS for timing validation if the GATE level models are time aware (delay annotated).

	
### Synthesis Simulation Mismatches

	-	Missing sensitivity list
	-	Blocking and non blocking statements
	
### Missing sensitivity list

Simulator functions on the basis of activity that is it looks for if either of the inputs change. If there is no change in the inputs the simulator won't evaluate the output at all.

Example:

module mux(input i0, input i1, input sel, output reg y);

always @(sel)

begin

	if (sel)
	
	begin
	
		y = i1;
		
	end
	
	else
	
	begin
	
		y = i0;
		
	end
	
end

endmodule

This mux code has a fault in that simulation only occurs while select is high, which means that if select is slow and there are changes in i0 or i1, they are completely ignored. Therefore, for the simulator, this mark is equivalent to a latch or a double H block, but the synthesizer constructs a mux based on functionality rather than sensitivity. 

### Blocking and non blocking statements Inside always block

	-	Blocking Executes the statements in the order it is written So the first statement is evaluated before the second statement
	-	Non Blocking Executes all the RHS when always block is entered and assigns to LHS. Parallel evaluation
	
### Example of Blocking assignments:

module shift_register(input clk, input reset, input d, output reg q1);

reg q0;

always @(posedge clk,posedge reset)

begin
	if(reset)
	
	begin
	
		q0 = 1'b0;
		
		q1 = 1'b0;
		
	end
	
	else
	
	begin
	
		q0 = d;
		
		q1 = q0;
		
	end
	
end

endmodule

In this case, D is assigned to Qo not which is then assigned to Q. Due to optimisation a single latch is formed where Q is equal to D.

### Example of Non-Blocking assignments:

In the above RTL code if

      begin
      
		q0 <= d;
		
		q1 <= q0;
		
      end
      
In the non blocking assignments all the RHS are evaluated and parallel assigned to lhs irrespective of the order in which they appear. So we will always get a two flop shift register.

Therefore we always use non blocking statements for writing sequential circuits.

Other example:

module comblogic(input a, input b, input c, output reg y);

reg q0;

always @(*)

begin

	y = q0 & c;
	
	q0 = a|b;
	
end

endmodule

Whenever any of the inputs, A, B, or C, changes, we enter the loop, but because Y is assigned the old Qo value because it is utilizing the value of the previous Tclk, the simulator imitates a delay or flop. On the other hand, the OR and AND gates are visible during synthesis as predicted.

Because of this, we should evaluate Q0 before evaluating Y when utilizing blocking statements in this scenario so that Y takes on the updated values of Qo. Even though both synthesised circuits produce the identical digital circuit made up of AND and OR gates. However, when we simulate, we see distinct behaviors.

# Labs on GLS and Synthesis-Simulation Mismatch

Example 1: A mux designed with the help of ternary operator

module ternary_operator_mux (input i0 , input i1 , input sel , output y);

	assign y = sel?i1:i0;
	
endmodule


![Capture1](https://user-images.githubusercontent.com/123365828/214826034-a3fb30fa-4e2f-4266-90b0-6997dde63618.PNG)

iverilog ternary_operator_mux.v tb_ternary_operator_mux.v 

./a.out

gtkwave tb_ternary_operator_mux.vcd 


![Capture2](https://user-images.githubusercontent.com/123365828/214826965-799b65c7-ddcd-4af7-9250-08eec785958c.PNG)


The synthesized netlist for the above,using yosys

![Capture3](https://user-images.githubusercontent.com/123365828/214828524-8ba5f49c-e389-4c3c-8168-49f17b2926f4.PNG)

To invoke GLS,

We need to read our netlist file and the test bench file assosciated with it.

We need to read two extra files that contain the description of verilog models in the netlist.

iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd

![Capture4](https://user-images.githubusercontent.com/123365828/214830101-635739e5-08eb-4ec1-89b2-a3b8223cf2d8.PNG)

The generated netlist does behave like a 2X1 multiplexer.

Example 2:

module bad_mux (input i0 , input i1 , input sel , output reg y);

always @(sel)

begin

	if(sel)
	
		y <= i1;
		
	else
	
		y <= i0;
		
end 

endmodule

![Capture5](https://user-images.githubusercontent.com/123365828/214830476-93086edc-1903-48be-bf61-475f51aec8f4.PNG)

In this example,javascriptalways is evaluated only if javascriptselect is high.It is insensitive to javascriptio or i1 because javascriptselect is low.

This is verified by GTKWAVE of RTL Simulation below :

![Capture6](https://user-images.githubusercontent.com/123365828/214831125-a6254a4f-68e1-4075-9b9f-06284bb95a84.PNG)

The design simulates a latch rather than a 2x1 mux.

But the Yosys implementation shows a 2X1 mux .

![Capture7](https://user-images.githubusercontent.com/123365828/214831754-8d1bf1ea-e32a-4be5-a423-7413da57468e.PNG)

iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd

If we now implement it's GATE level netlist through GLS and observe the waveform,it shows the behaviour of a 2X1 mux as shown below:

![Capture8](https://user-images.githubusercontent.com/123365828/214832368-bff9efaf-b422-4351-9915-759e904ac1b5.PNG)

Since,the waveforms of stimulated RTL Code : Is of a LATCH the waveforms of gate level netlist thruogh GLS after synthesis: Is of 2X1 MUX We see a Synthesis-Simulation Mismatch.

Example 3: This is an example of synthesis-simulation mismatch due to wrong order of assignment in blocking assignments.

module blocking_caveat (input a , input b , input c , output reg d);
reg x;

always @(*)
begin
	d = x & c;
	x = a | b;
end 
endmodule

vim blocking_caveat.v

![Capture9](https://user-images.githubusercontent.com/123365828/214832948-aa5eb22f-9a70-4248-9837-a164c021611b.PNG)

We enter into the loop whenever any of the inputs a b or C changes but D is assigned with old X value since it is using the value of the previous Tclk ,the simulator mimics a delay or a flop. Where as, during synthesis we see the the OR and AND gates as expected.

![Capture10](https://user-images.githubusercontent.com/123365828/214833646-baf40924-1a6e-4338-bd46-25a7e8c4577b.PNG)

a | b should produce 0 when both inputs a and b are 0, which, when ANDed with c, should produce an output y of 0. Thus, the output y ought to have the number 0. It currently has the value 1, though. However, x really holds the value of an OR b from the preceding clock because to the blocking statements in the rtl function, giving us an inaccurate output.

The netlist representation on synthesis yields

![Capture11](https://user-images.githubusercontent.com/123365828/214834249-b0a9c473-6365-4642-896f-112dc877a3da.PNG)

The RTL design's functionality is what the synthesizer sees instead of the sensitivity list. As a result, there are no latches in the netlist representation to store delayed values from the preceding cycle. There is simply an OR 2 AND gate present.

iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd

If we run gate level simulations on this netlist in verilog, we observe the following waveform.

![Capture12](https://user-images.githubusercontent.com/123365828/214835929-17f94b84-32d6-47a2-ac4f-b106f2d2573a.PNG)


Here, we can see that the circuit operates as intended combinational ckt. In contrast to the simulation results, output d is produced using the current value of the inputs rather than the past clock values. We experience a Synthesis-Simulation Mismatch once more because the waveforms of the stimulated RTL verilog code do not match the gate level simulation of the generated netlist.

# Day 5 - If, Case, For Loop and For Generate

### If Constructs

If condition is used to to write priority logic. The condition one has a priority or if has more priority than the consecutive else statements . Only when condition 1 is not met condition 2 is evaluated and so on and y is assigned accordingly depending on the matching conditions.

	if (cond_1)

	begin 	

		y = statement_1;

	end

	else if (cond_2)

	begin

		y = statement_2;

	end

	else if (cond_3)

	begin

		y = statement_3;

	end
	else
	begin
		y = statement_4;
	end
	
If-else block implements a Priority logic that is if cond_1 is satisfied, next if statements are not executed. Thus above If-Else code translates to a ladder like multiplexer structure in the final design instead of a single multiplexer.

Dangers of using Incomplete If statements Inferred Logic which occurs due to bad coding styles that is incomplete if statements.

	if (condt1) 
	    y-a;
	else if (condt2)
	    y=b;
	    
In the above code if condition 1 is matched y is equal to a else if condition 2 is matched y is equal to b but there is no specification for the case when condition2 is not matched, as a result of which the simulator tries to latch this case to the output y.It wants to retain the value of y.
This is a combinational loop to avoid that the simulator infers a latch. Enable of this latch is OR of the condition 1 and condition 2. If neither condition 1 or condition 2 is met the OR gate output disables the latch . The latch retains the value of y and stores it.
This is called the inferred latch due to incomplete if statements which is very dangerous for RTL designing. It should be avoided except for some special cases like the counter.

	reg[2:0]
	always @(posedge clk,posedge reset)
	Begin
	If (reset)
	Count<= 3'b000;
	else if(enable)
	Count<= count+1;
	end
	
This is also a case of incomplete if statements. Here ,if there is no enable the counter should latch onto the previous value.For example if the counter has counted up till 4 and there is no enable then it should retain the value 4 rather than going to 0 again.
So here the incomplete if statements result in latching And retaining the previous value which is our desired behavior in a counter. The earlier mux example was a combinational circuit and therefore we cannot have inferred latches.

Note:
If, case statements are used inside always block.
In verilog whatever variable we use to assign in if or case statements must be a register variable.

### CASE Constructs

Let's look at the following verilog code block. Here, the inferred hardware should be a 4:1 multiplexer. The CASE statements do not have priority logic like IF statements.

	always @(*)
	begin
	     case(sel)
			2'b00: begin
				y = statement_1;
				end
			2'b01: begin
				y = statement_2;
				end
			2'b10: begin
				y = statement_3;
				end
			2'b11: begin
				y = statement_4;
				end
		endcase
	end
Depending on the cases matching the select y is assigned accordingly.

Some caveats with using CASE statements:

### 1.Incomplete CASE

Let's say I have a two bit variable select.

	reg [1:0] sel
	always @(*)
	begin
	     case(sel)
	     2'b00: begin
			   . condition 1
			   end
	     2'b01: begin
			   . condition 2
			   end
	    end case
	    end
	    
Then select is C1 or C3 the conditions are not specified. It causes an incomplete case which results in inferred latches for these two cases that latch on to output y.This occurs when some cases are not specified inside the CASE block. For example, if the 2'b10 and 2'b11 cases were not mentioned , the tool would synthesize inferred latches at the 3rd and 4th inputs of the multiplexer.
Solution is code case with default inside the CASE block so that the tool knows what to do when a case that is not specified occurs.

	Correct code :

	reg [1:0] sel
	always @(*)
	begin
	     case(sel)
	     2'b00: begin
			   . condition 1
			   end
	     2'b01: begin
			   . condition 2
			   end
	    default :begin
			   . condition 3
			   end   
	    end case
	    end
	    
### 2.Partial assignments

	always @(*)
	begin
		case(sel)
			2'boo: begin
				x = a;
				y = b;
			2'b01: begin
				x = c;
			default: begin
				x = d;
				y = d;
				end
		endcase
	end
	
In the above example, we have 2 outputs x and y. This will create two 4X1 multiplexers with the respective outputs. If we look at case 2'b01, we have specified the value of x for this case ,but not the value of y. It appears that it is okay to do so, as a default case is specified for both the outputs, and if we don't directly specify the value of y for any case, the simulator will implement the default case. This, however , is incorrect. In partial assignments such as this, the simulator will infer a latch at the 2nd input for multiplexer y as no value is specified for a particular case.

### 3.Overlapping cases

	always @(*)
	begin
		case(sel)
			2'b00: begin
				y = a;
			2'b01: begin
				y = b;
				end
			2'b10: begin
				y = c;
			2'b1?: begin
				y = d;
				end
		endcase
	end
	
In the above code block ,2'b1? specifies that the corresponding bit can be either be 0 or 1. This means when the sel input is holding a value 3 i.e 2'b11, cases 3 and 4 both hold true. What is synthesized depends on the mercy of the simulator. It can lead to Synthesis-Simulation mismatches.
If we used an IF condition here, due to priority logic, condition 4 would be ignored when condition 3 is met. However,in the CASE statement , even if the upper case is matched,all the cases are checked.So,if there is overlapping in cases,it poses a problem as the cases are not mutually exclusive. And we would get an unpredictable output.

### Labs on Incorrect IF and CASE constructs

Example 1: Incomplete If statements

Below is the file titled incomp_if.v, and can be found in the directory verilog_files.

	module incomp_if(input i0 , input i1 , input i2 , output reg y);
	always @(*)
	begin
		if(i0)
			y <= i1;
	end
	endmodule
	
The code contains an incomplete IF statement as no else condition corresponding to it is mentioned . On simulating this design , following gtkwave is obtained

![image](https://user-images.githubusercontent.com/123365828/215085842-2f8852f8-b5e0-4bca-ab1f-1cb76f792d71.png)

From the above waveform, We observe no change in y when i0=0.It's equal to previous value when io=0. This shows latching Action, which is verified by looking at the synthesis implementation using Ysosys.

![Capture2](https://user-images.githubusercontent.com/123365828/215086747-6a2f43bb-90f8-471c-8189-82a6b26b980b.PNG)

We see a D latch is created in the synthesized netlist.

Example 2:

Below is a similar example of incomp_if2.v

	module incomp_if2(input i0 , input i1 , input i2 , input i3,  output reg y);
	always @(*)
	begin
		if(i0)
			y <= i1;
		else if (i2)
			y <= i3;

	end
	endmodule

The above code contains an incomplete IF statement as well. Here, we have 2 inputs i1 and i3, as well as 2 conditional inputs i0 and i2. As we do not specifythe case when both i0 and i2 go low,which results in an issue in the synthesis. The gtkwaveform of the simulated design is below

![Capture3](https://user-images.githubusercontent.com/123365828/215088260-3fe8f632-9f6c-4066-981d-07bcd13d2a89.PNG)

Observation: When io is high,output follows i1. When io is low,it looks for i2.If i2 is high,it follows i3. But if i2 is low(and io is already low),y attains a constant value=previous output.

This can be verified by checking the graphical realisation of the yosys synthesis below.

![Capture4](https://user-images.githubusercontent.com/123365828/215089148-e23d7468-70a7-4dff-afcb-868cea458f9c.PNG)

Yosys synthesizes a multiplexer as well as a latch with some combinational logic at its enable pin.

Example3:

Below is a design with Incomplete Case's specification in a mux.

	module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel , output reg y);
	always @ (*)
	begin
		case(sel)
			2'b00: y = i0;
			2'b01: y = i1;
		endcase
	end
	endmodule

Whenever se[1]=1 ,latching action takes place. The yosys synthesis implementation is given below.

![Capture5](https://user-images.githubusercontent.com/123365828/215090040-4874056d-db97-4231-b31c-8ca95d0305b5.PNG)

Observation: 1. !(sel[1]) is going to D latch enable. 2.The inputs io,sel[0], !(sel[1]) go to the upper mixing logic that is implemented on D pin of the latch.

Example 4:

Complete Mux along with all the cases specified.

	module comp_case (input i0 , input i1 , input i2 , input [1:0] sel , output reg y);
	always @ (*)
	begin
		cae(sel)
			2'b00: y = i0;
			2'b01: y = i1;
			default:y = i2;
		endcase
	end
	endmodule

Output follows i2 at default case,if i1 and io go low. Hence a 4X1 mux is synthesized without any latch that can be verified below.

![Capture7](https://user-images.githubusercontent.com/123365828/215091221-df5d1c71-fbd4-450f-a36c-80b2edd893c7.PNG)

Example 5: Partial Assignments

	module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel , output reg y , output reg x);
	always @ (*)
	begin
		cae(sel)
			2'b00: begin
				y = i0;
				x = i2;
				end
			2'b01: y = i1;
			default: begin 
				x = i1;
				y = i2;
				end
		endcase
	end
	endmodule

Expected Circuit: The 2X1 mux with output y is inferred without any latch. 

Condition for enabling:

		      en=sel[1]+!(sel[0]) 

Using Redundancy Theorem

Yosys implementation of the above design after synthesis,

![Capture8](https://user-images.githubusercontent.com/123365828/215092117-41921581-d182-430d-853b-104f932387c3.PNG)

As expected, Only 1 D latch is inferred for X. No latch is inferred for y.

Example 6: Design of 4X1 mux having overlapping cases

	module bad_case (input i0 , input i1 , input i2 , input [1:0] sel , output reg y);
	always @ (*)
	begin
		cae(sel)
			2'b00: y = i0;
			2'b01: y = i1;
			2'b10: y = i2;
			2'b1?: y = i3;

		endcase
	end
	endmodule

In gtkwaveform of RTL simulation:

![Capture9](https://user-images.githubusercontent.com/123365828/215093220-91115ce6-5ce8-4cd5-89ea-aa4c2432503e.PNG)

Observation : When sel[1:0]=11, the output neither follows i2 nor i3. It simply latches to 1.

Whereas while running GLS on the netlist,the waveform of the synthesized netlist behaves as 4X1 mux as shown below

![Capture10](https://user-images.githubusercontent.com/123365828/215098579-0d02086a-e781-4baf-a006-ac9e14ea38ce.PNG)

Thus ,Overlapping cases confuse the simulator and leads to Synthesis-Simulation Mismatches.

### Introduction to Looping Constructs

There are two types of FOR loops in verilog.

	-FOR loop 1.Used within the always block 2.Used to evaluate expressions

	-Generate FOR loop 1.Only used outside the always block 2.Used for instantiating hardware

Necessity of FOR loops

For loops are extremely useful when we want to write a code /design that involves multiple assignments or evaluations within the always block. Lets us take an example: If we want to write the code for 4:1 multiplexer, we can easily do so using a either four if blocks or using a case block with 4 cases,as seen in the previous if-else blocks.But this approach is not suitable for complicated design with numerous inputs/outputs say 256X1 mux.If we wanted to design a 256X1 multiplexer, we will have to write 256 lines of condition statements using select and corresponding assignments. But in for loop ,be it 4X1 or 256X1 we would always be writing 4 lines of code only. Although we need to provide 256 inputs using an internal bus.

	integer k;
	always @(*)
	begin
		for (k = 0, k < 256, k= i  +1)
		begin
			if (k== sel)
				y = in[i];
			end
		end
	end

This code can be infinitely scaled up by just replacing the condition i < 256 with the desired specification for our multiplexer.

Similarly, we can create High input demultiplexers as well.

	integer k
	always @(*)
	begin
		int_bus[15:0] = 16b'0;
		for (k= 0; k< 16; k= k+ 1) 
		begin
			if (k == sel)
				int_bus[k] = inp[k];
			end
		end
	end

Here , we have created a 16:1 demultiplexer using for loops within the always block. The int_bus[15:0] specifies our internal bus which takes on the input of the demux. It is necessary to assign all outputs to low for a new value of sel else latches will be inferred resulting in the incorrect implementation of our logic.

Below are few of the examples of FOR construct .

Example 1:

file mux_generate.v that generates a 4X1 mux using For loop.

	module mux_generate (input  i0, input i1 , input i2 , input i3 , input [1:0] sel , output reg y);
	wire [3:0] i_int;
	assign i_int = {i3,i2,i1,i0};
	integer k;

	always @(*)
	begin
		for(k = 0; k < 4; k=k+1)begin
			if(k == sel)
				y = i_int[k];
		end
	end
	endmodule

The 4 inputs get assigned to a the internal 4 bit bus named i_int.

The gtkwave obtained after the simulation

![Capture11](https://user-images.githubusercontent.com/123365828/215106971-741e1c97-febe-4248-ba9d-bd8c2645da93.PNG)

Example 2:

Similar to example 1, file demux_generate.v that generates a 4X1 demux using For loop.

	module demux_generate (output o0 , output 02 , output o3 ,  output o4 , output o5 , output o6 , output o7 , input [2:0] sel , input i);
	reg [7:0]y_int;
	assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
	integer k;

	always @(*)
	begin
		y_int = 8'bo;
		for( k = 0; k < 8; k++) begin
			if(k == sel)
				y_int[k] = i;
		end
	end 
	endmodule

The above code has good readabilty,scalability and easy to write as well. Let's verify if it functions as a 8X1 demux as expected by viewing its gtkwave simulated waveform.

![Capture12](https://user-images.githubusercontent.com/123365828/215108332-680fdca3-cafd-4a20-943b-5f9dfbee0814.PNG)

### FOR Generate and its Uses

FOR Generate is used when we needto create multiple instances of the same hardware. We must use the For generate outside the always block.

We take example of a 8 bit Ripple Carry Adder(RCA) to understand the ease of instantiations provided by the For generate statement. An RCA consists of Full Adders tied in series where the carry out of the previous full adder is fed as the carry in bit of the next full adder in the chain. Hence, we can make use of generate for to instantiate every full adder in the design , as they are all represent the same hardware.

For this example , we use the file rcs.v which holds the code for the ripple carry adder. It also needs to be included in our simulation.

	module rca (input [7:0] num1 , input  [7:0] num2 , output [8:0] sum);
	wire [7:0] int_sum;
	wire [7:0] int_co;

	genvar i;
	generate
		for (i = 1; i < 8; i=i+1) begin
			fa u_fa_1 (.a(num1[i]),.b(num2[i],.c(int_co[i-1],.co(int_co[i]),.sum(int_sum[i]));
		end

	endgenerate
	fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));

	assign sum[7:0] = int_sum;
	assign sum[8] = int_co[7];
	endmodule

Here, fa references another verilog design file containing the definition of for the full adder submodules .This is shown below, from the fa.v file

	module fa(input a, input b , input c,  output co, output sum);
		assign  {co,sum} = a +b + c;
	endmodule

In the RCA verilog code, we instantiate fa in a loop using generate for outside the always block.

Rules for addition :

N + N bit number --> Sum will be N + 1 bits N +M bit number --> Sum will be max(N,M) +1 bits

Now, let us simulate this design in verilog and view its waveform with GKTWave .As the rca design referances the file fa.v , we must specify it in our commands as follows

iverilog fa.v rca.v tb_rca.v
./a.out
gtkwave tb_rca.v
the resulting gtkwaveform is shown below that shows an adder being simulated:

![Capture13](https://user-images.githubusercontent.com/123365828/215110635-da6e4609-15ed-41d8-82ed-8a3a9be8728b.PNG)


Acknowledgment:

1.Kunal Ghosh - Co-founder(Vsd corp. pvt.ltd.)

2.Shon Taware

#  Day 6 Content

#  Introduction to FPGA Programming

## Counter

### Simulation Result

![image](https://user-images.githubusercontent.com/123365828/215696478-69e282e3-4c70-4a26-81ee-e7b76d9086e1.png)

![image](https://user-images.githubusercontent.com/123365828/215696554-db0acbaa-8d8e-46c9-9ff4-0ec58e59407e.png)

### RTL Schematic Design

![image](https://user-images.githubusercontent.com/123365828/215696951-b4901b9d-51ee-4f8e-bb24-0a24aa20a6cb.png)

### Synthesis Design:

![image](https://user-images.githubusercontent.com/123365828/215697096-4389392b-df00-467f-baf9-3b79a3c3b0c2.png)

![image](https://user-images.githubusercontent.com/123365828/215697140-b8f97853-4e46-4fd3-b597-b80302e1c904.png)

### Constraints

![image](https://user-images.githubusercontent.com/123365828/215697259-6e67ab94-19a1-4541-97d7-b29fe9d15673.png)


### Report Timming Summary

### Constraints are met

![image](https://user-images.githubusercontent.com/123365828/215697600-d7ef1c0a-2813-46de-ae4b-cd1f2e1e1803.png)

### Report Utilization

![image](https://user-images.githubusercontent.com/123365828/215698198-ef4bc6ee-be16-44e8-9642-1365a29ec6ea.png)

### Mapping 

![image](https://user-images.githubusercontent.com/123365828/215698327-125a3167-495d-4b0c-bd21-9cb1097a79e4.png)

### Generate Bit Stream and write to target device (Bassys 3 Board)

![image](https://user-images.githubusercontent.com/123365828/215698878-db19dd3a-da3b-4064-958b-30c71f41e632.png)

![image](https://user-images.githubusercontent.com/123365828/215698938-2891d59a-1ec3-4976-a663-840063527c9c.png)

#### warning: The debug hub core was not detected at User Scan Chain 1 or 3.

## VIO

![image](https://user-images.githubusercontent.com/123365828/215699521-339089f3-2c62-45dd-9439-875709f45891.png)

![image](https://user-images.githubusercontent.com/123365828/215699777-c4da3914-1de6-40b7-85d2-aea212fe7b2f.png)

#### Error

![image](https://user-images.githubusercontent.com/123365828/215699864-f81fa69c-2a71-4b34-b7e3-814e59eacc2b.png)

![image](https://user-images.githubusercontent.com/123365828/215699953-b4f0865f-351a-43f0-8aee-b37dab6ad3ef.png)

# Day 7

## Inception of open-source EDA, OpenLANE and sky130 PDK

### How to talk to computers?

To perform the intruction, which are given externaly in computers, some hardware is there which convert our external instruction into understandable language of computers. In this hardware, many things are there. For example FPGA board, Arduino board etc. are provides the medium for external inputs and outputs.

Taking the example of Aurdino board. Arduino consists of both a physical programmable circuit board (often referred to as a microcontroller) and a piece of software, or IDE (Integrated Development Environment) that runs on the computer, used to write and upload computer code to the physical board.

compontents on arduno Here in the board, the ATmega328 microcontroller is used. Basically microcontroller is made from package, inside the package chip is there.Chip is made by pads, core, die and foundary IP's.

Package

The package is a case that surrounds the circuit material to protect it from corrosion or physical damage and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). here what we are seeing in the black color is the Package of the microcontroller.


Chip

Inside the Package, chip is available which contains, foundary IP's (for example, RISC-V Soc, PLL, ADC, DAC, SRAM, SPD), core (core contains AND, OR etc. all gates and digital logics), pads (through which input and output signals are communicates).


What is RISC-V?

RISC-V, where five refers to the number of generations of RISC architecture that were developed at the University of California, Berkeley. RISC is an open standard instruction set architecture (ISA) based on established RISC principles. Unlike most other ISA designs, RISC-V is provided under open source licenses that do not require fees to use. A number of companies are offering or have announced RISC-V hardware, open source operating systems with RISC-V support are available, and the instruction set is supported in several popular software toolchains.

The instruction set is designed for a wide range of uses. The base instruction set has a fixed length of 32-bit naturally aligned instructions, and the ISA supports variable length extensions where each instruction can be any number of 16-bit parcels in length. The instruction set specification defines 32-bit and 64-bit address space variants. The specification includes a description of a 128-bit flat address space variant, as an extrapolation of 32 and 64 bit variants, but the 128-bit ISA remains "not frozen" intentionally, because there is yet so little practical experience with such large memory systems.

### How software communicate with Hardware?

Between apps or software (in our mobilephones and computers or laptops) and Hardware (in our mobilephones and computers or laptops), One full channel is there, which contains O.S. (operating system), compiler and assembler. This channel proccesses the inputs from the apps and gives the outputs to the Hardware. And according to the output of assembler, the hardware will perform.

when we gives the inputs from the software, inputs is in the C,C++,JAVA etc. languages. Hardware not understand this language. So, this inputs goes throuth the system software cahnnel, where O.S. handles the input/output operations, it allocates the memory to for the codes and low level systems functions are there in O.S. Then program goes to the compiler, where program will compile and generate the HDL program.This HDL program is basically the sets of Instructions which can hardware is able to understand. The instructions are basically depends on what kind of hardware we are using. For example if we are using Mips processor then instruction formaate will be according to the Mips processor, if Hardware is RISC-V then the instruction formate will be according to the RISC-V and similerly for Arm and intel processors also.

This sets of instrusctions is now given to the Assembler. here according to the instruction set, assembler will generate the Binary code (contains only 0s and 1s). this binary code can be understandable for hardware. And according to this code, hardware will gives the output.

## How to design Digital ASIC?

To design Digital ASIC, few tools or things which are required from the day one. These are

	-RTL Design
	-EDA tools
	-PDK data
	
what is RTL design?

In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.for this designs many open sorces are available. like, librecores.org, opencores.org, github.com, etc...

What is EDA tools?

The term Electronic Design Automation (EDA) refers to the tools that are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. many open sorces tools are available like Qflow, OpenROAD, OpenLANE, etc...

What is PDK Data?

PDK is process design kit. It is interface between FAB and design. This data is collections of files like,

	-process design rules: DRC, LVS, REX
	-Digital standerd cell libreries
	-i/o librerirs
	-etc.....
	
which are used to model a fabrication process for the EDA tools used to design an ICs. for example, in 2020, google release the open source PDK for FOSS 130nm production with the skywater technology. But right now it is at cutting age of the 5 nm also. But in many applications, the advance node is not required, and the cost of advanced node is also high as compared to 130nm processors. This 130nm processors are also fast processor. for example,

intel: P4EE @3.46 GHz(Q4'o4)

sky130_OSU (single cycle RV32i CPU) pipeline version can achieve more than 1 GHz clock

## Simplified RTL to GDSII flow

### Floor planning and Power planning

The main objective here is that to plan silicon area and distribute the power to the whole circuit. In the chip floor planning, the partition chip die between different system building blocks and place the i/o pads. In micro floor planning, we define the dimensions, pin locations, rows.

![image](https://user-images.githubusercontent.com/123365828/216031368-52b4e3e3-cd29-47a5-9904-65d5d11d071b.png)

### Placement

In this process, we place the gate level netlist on the floor planning rows, alligned with the sites. cells should be placed very closed to eachother to reduce the interconnnect delay. Usually placement is done in 2 steps:

	- Global placement
	- Detailed placement
	
![image](https://user-images.githubusercontent.com/123365828/216048219-c58a0535-c300-46c3-aa99-e1ef431ed905.png)
	
### Global placement

Global placement is very first stage of the placement where cells are placed inside the core area for the first time looking at the timing and congestion. Global Placement aims at generating a rough placement solution that may violate some placement constraints while maintaining a global view of the whole Netlist.

### Detailed placement

In detailed placements, we determined the exact route and layers for each netlist. the objective of detailed placement is valid routing, minimize area and meet timing constrains. Additional objective is minimum via and less power.

### Clock tree synthesis (CTS)

Before routing the signals, we have to route the clock. In the process of clock synthesis, we have distribute the clock to the every sequential elements. for example flipflops, registers, ADC, DAC ete. basically clock netwroks looks likes a tree. where the clock source is roots and the clock elements are end leaves. Synthesization should be done in a manner that with minimum skew and in a good shape.To minimize the clock skew by using the low-skew global routing resources for clock signals.Microsemi devices provide various types of global routing resources that significantly reduce skew.Usually a tree is a H tree, X tree etc.

![image](https://user-images.githubusercontent.com/123365828/216039808-c684af23-b04b-4bc1-9011-f0b552eac943.png)

### Routing

After routing the clock, the signal routing comes. Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined. There are two types of nets in VLSI systems that need special attention in routing:

	Clock nets
	Power/Ground nets
	
The sky130 PDK defines the 6 routing leyers. the lowest leyer is called local interconnect layer (titanium nitride layer). Other five layers are alluminium layers.

![image](https://user-images.githubusercontent.com/123365828/216039945-526a9525-3ba9-41be-88e9-be923dce8132.png)

In the proccess of routing, metal trackes forms a routing grids and these grids are huge. so, devide and conquer approach is use for routing. The two types of routing is used:

	- Global routing: Generates the routing guides
	- Detailed Routing: Uses the routing guides to implement the actual wiring
	
### Sign off

Once the routing is done, we can construct the final layout. This final layout will goes under the verification. Two types of verifications are there:

	- Physical verification: Here design rule checking will done and it will check the final layout and owners layout
	- Timing Verification: Here Static Timing Analysis will done


## Introduction to Openlane

OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII - this capability will be released in the coming weeks with completed SoC design examples that have been sent to SkyWater for fabricaiton. It is started as an Open-source flow for a true Open Source tape-out Experiment. striVe is a family of open everything SoCs:

	- Open PDK
	- Open EDA
	- Open RTL
	
### striVe SoC Family

![image](https://user-images.githubusercontent.com/123365828/216040755-dcce56f8-f62b-4146-9aae-3c04a83b8de1.png)

The main goal of OPENLANE is to produce a clean GDSII with no human intervation (no-human-in-the-loop). here the meaning of clean is that:

	- No LVS violations
	- No DRC Violations
	- No timing Violations

OPENLANE is tuned for skyWter130nm open PDK. it can be used to harden Macros and chips.there is two mode of operation

	- Autonomus : it is the push botton flow. with the push botton , it is a some time base design and due to this push botton, we get final GDSII
	- interactive : here we can run comamds and steps one by one.

It has large number of design examples(43 designs with their best configurations).


### OpenLANE Architecture

![image](https://user-images.githubusercontent.com/123365828/215985039-63ee7157-ff08-456a-95c1-ec70c6f66458.png)

### DFT(Design for Test)

it perform scan inserption, automatic test pattern generation, Test patterns compaction, Fault coverage, Fault

![image](https://user-images.githubusercontent.com/123365828/216032080-bd3ca425-a6fb-42a3-8b5e-cd17d620003d.png)

After that physical implementation is done by OpenROAD app. physical implementation involves the several steps:

	- Floor/Power Planning
	- End Decoupling Capacitors and Tap cells insertion
	- Placements: Global and Detailed
	- Post Placement Optimization
	- Clock Tree synthesis (CTS)
	- Routing: Global and Detailed
	
Every time the netlist is modified.(CTS modifies the netlist and Post Placements optimization also modifies the netlist).so for that verification must be performed. The LCE(yosys) is used to formally confirm that the function did not change after modifying the netlist. ### Dealing with antenna rules Violation: when a metal wire segment is fabricated, it can act as antenna.as an antenna, it collect charges which can demaged the transister gates during the fabrication.

![image](https://user-images.githubusercontent.com/123365828/216047532-f7f569ac-68de-45eb-a7d8-450111f35f5a.png)

To address this issue, we have to limit the lenght of the wire. usually this is the job of the router. If router fails to do this, then there are two solutions:

	- Bridging attaches a higher layer intermediary
	- Add antenna diode cell to leak away charges.(Antenna diodes are provided by the SCL)
	

![image](https://user-images.githubusercontent.com/123365828/216047461-f90b1d7b-61e6-450a-ae4b-5afe3f12bd50.png)

![image](https://user-images.githubusercontent.com/123365828/216047418-be58c8a9-c52f-4165-b2b0-3c807b865dc2.png)

With OpenLANE, we took a preventive approach. here we add fake antenna diode next to every cell input after placement. Then run the Antenna checker on the routed layout. If the checker reports a violation on cell input pin, replace the fake diode cell by a real one.


![image](https://user-images.githubusercontent.com/123365828/216047361-4c3d80ed-828f-467d-b14b-8d2be0bc386e.png)### Static Timing analysis(STA)

It involves the interconnect RC Extraction(DEF2SPEF) from the routed layout, followed by STA on OpenSTA(OpenROAD) tool. resulting report will shows the timing violations if any violations is there.

### Physical Verification (DRC and LVS)

Magic is used for design Rules checking and SPICE Extraction from Layout. Magic and Netgen are used for LVS.

### OpenLANE Design Stages

OpenLANE flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLANE can also be run interactively which will be shown below.

#### Synthesis

	yosys - Performs RTL synthesis
	
	abc - Performs technology mapping
	
	OpenSTA - Pefroms static timing analysis on the resulting netlist to generate timing reports
	
#### Floorplan and PDN

	init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
	ioplacer - Places the macro input and output ports
	pdn - Generates the power distribution network
	tapcell - Inserts welltap and decap cells in the floorplan
	
#### Placement

	RePLace - Performs global placement
	Resizer - Performs optional optimizations on the design
	OpenDP - Perfroms detailed placement to legalize the globally placed components
	
#### CTS

	TritonCTS - Synthesizes the clock distribution network (the clock tree)
	
#### Routing *

	FastRoute - Performs global routing to generate a guide file for the detailed router
	TritonRoute - Performs detailed routing
	
#### GDSII Generation

	Magic - Streams out the final GDSII layout file from the routed def
	
#### Checks

	Magic - Performs DRC Checks & Antenna Checks
	Netgen - Performs LVS Checks
	
#### OpenLANE Output

All output run data is placed by default under ./designs/design_name/runs.


#### Flow configuration
	
	-PDK / technology specific
	-Flow specific
	-Design specific

#### Interactive Mode

	You may run the flow interactively by using the -interactive option:

	./flow.tcl -interactive
	
A tcl shell will be opened where the openlane package is automatically sourced:

	% package require openlane 0.9
	
Then, you should be able to run the following commands:

Any tcl command.
	
prep -design <design> -tag <tag> -config <config> -init_design_config -overwrite similar to the command line arguments, design is required and the rest is optional
	
	run_synthesis
	run_floorplan
	run_placement
	run_cts
	run_routing
	run_magic
	run_magic_spice_export
	run_magic_drc
	run_netgen
	run_magic_antenna_check
	
The above commands can also be written in a file and passed to flow.tcl:

	./flow.tcl -interactive -file <file>
	

### Open source EDA tools

OpenLANE Directory Structure in detail

cd command

cd means change directory. this command will help us to go inside the directory.

ls command

ls means listing the directory. It is used to find the list of total details of directory.

ls --ltr

This command will help to list the details in cronological order.

Here we are working in Sky130_fd_sc_hd PDK varient. where, "sky130" is process name or node name."fd" is a foundary name (skyWater foundary)."sc" means standerd cell librery files and the last one "hd" stands for high density(basically one type of varient).

Sky130_fd_sc_hd varient contains many technology files like verilog, spice, techlef, meglef,mag,gds,cdl,lib,lef,etc. (techlef file contains the layer information).

### Design Preparation Step

Running OpenLANE

Issue the following command to open the docker container from path/to/openlane to ensure that the output files persist after exiting the container:

	   docker 
	   
Use the following example to check the overall setup:

	./flow.tcl -interactive
	
when we enter in the OpenLANE, we have to use flow.tcl because as a name says, it will goes with the flow using the script. And by using interactive switch, we will do step by step process. without interactive switch, it will run complete flow from RTL to GDSII. Now OpenLANE is open and we can see that prompt will change now.

![Capture1](https://user-images.githubusercontent.com/123365828/216033256-e988a1ed-0b38-4e7a-a7a3-c0f3e7350f61.PNG)
	
	Now we have to input all the packages which required to run the flow.
	
![Capture1](https://user-images.githubusercontent.com/123365828/216034929-5540102e-6935-49fe-b04e-4ffe827aaa21.PNG)
	
Now, here we are ready to execute the command.

Now, if we are going into the design folder in openlane, there are nearly 30-40 designs are already builted. Out of them we can open any of the design. for example, here we are opening the picorv32a.v design. In this design we can see many files are available. i.e., scr, config.tcl, etc. This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc.

![Capture3](https://user-images.githubusercontent.com/123365828/216035192-eebed710-14f9-4608-82a4-09d7879c0983.PNG)

Here we can see that the time period is set to the 5.00 nsec. but is we see in the openlane sky130_fd_sc_hd folder, the period is set about 24 nsec. so it is not override to the main file. If it override then give first priority to the main folder.
	
![Capture4](https://user-images.githubusercontent.com/123365828/216035349-c34c969d-de55-48e0-9502-e57b98f35060.PNG)

Now, in openlane, we are going to run the synthesis, but before synthesis, we have to prepare design setup stage. for that command is " prep -design picorv32a".
	
![Capture2](https://user-images.githubusercontent.com/123365828/216035511-30977613-f845-4189-a75b-80b99e2c5061.PNG)

so, here it is shown that preparation is completed.

### Review after design preparation and run synthesis
	
After completing the preparation, in the picorv32a file, the run terictory is created. Inside the folder, Today's date is created. so in this terictory some folders are available which is required for openlane.

![Capture6](https://user-images.githubusercontent.com/123365828/216036303-b4854a28-17ad-4c22-82c1-68adfa94f76e.PNG)
	
In the temp file, merged.lef file is available which was created in preparation time. if we open this merged.lef file, we get all the wire or layer level and cell level information.

![Capture7](https://user-images.githubusercontent.com/123365828/216036466-6791a7e1-7679-4b8c-a930-42b83f286393.PNG)
	
![Capture8](https://user-images.githubusercontent.com/123365828/216036569-c5458461-be74-4bdb-a1c3-98d6ee9142e9.PNG)
	
While, in the result folder is empty because till we have not run anything and in the report folder all the folders are there about synthesis, placement, floorplanning,cts,routing,magic,lvs.
	
![Capture9](https://user-images.githubusercontent.com/123365828/216036704-8ee82557-b9aa-4d5a-9b36-bcfb61848541.PNG)
	
now here also one config.tcl file is available similar like design folder. But this config.tcl file contains all default parameter taken by the run.

![Capture10](https://user-images.githubusercontent.com/123365828/216036885-44363a32-317e-4c95-879e-0231e73988cb.PNG)

when we make some change in the origional configuration and then we run, for example if we make a change in core utilization in the floorplanning and then we run the floorplanning, at this time in the congig.tcl file, the core utility will change and by cross checking it we can check that the modification is reflected in the exicution or not.

Now, cmds.log file takes all the record of the commands, what we have fab.
	
![Capture11](https://user-images.githubusercontent.com/123365828/216037000-ca3575ba-12ac-4eef-ba0f-5a64b2fd4d50.PNG)

Now coming to the openlane, we are going to run the synthesis. It will take some 3-4 mnts to run the synthesis and finally synthesis will complited.

![Capture12](https://user-images.githubusercontent.com/123365828/216037107-e992c076-e669-4d22-9bc8-f80bff2c7f52.PNG)

From the data of synthesis, total number of counter D_flip-flops is 1613. and the number of cells is 14876.

![Capture13](https://user-images.githubusercontent.com/123365828/216037904-572e1de7-8ee3-4eae-b775-33a12c857ab2.PNG)

So, the flop ratio = (number of flip flops)/(number of total cell).

So, the flop ratio is 10.84%.

Before run, we saw that the result folder is empty. but now, after running the synthesis, we can see that all the mapping have been done by ABC.

![Capture14](https://user-images.githubusercontent.com/123365828/216038091-ce7afa21-2dde-4018-9ba0-25f9ac8726ea.PNG)

And in the report, we can see when the actual synthesis has done. and the actual statistics synthesis report is showing below, which is same as what we have seen before.
	
![Capture15](https://user-images.githubusercontent.com/123365828/216038212-65fa97b7-f9e1-4c7a-8c81-3ad6c9048ffe.PNG)
	
# Day 8

# Good floorplanning Vs Bad floorplanning and introduction to library cells
	
## Chip floorplanning considerations
	
### Utilization factor and aspect ratio
	
#### 1. defining the width and height of core and Die
	
![image](https://user-images.githubusercontent.com/123365828/216042468-94a57fa4-24c7-43f8-ac17-88e1696673ad.png)

let's begin with netlist( Netlist describes the connectivity of an electronic designs). Considering a netlist with 2 flops and 2 gates.
	
![image](https://user-images.githubusercontent.com/123365828/216042560-406bcd4c-494d-4d73-8a1f-680c906d40aa.png)

lets giving the height and width of standerd cell is 1 unit. So, area of standerd cell is 1 square unit. And assuming the same area for the flip flops also.

Now lets calculate the area occupied by the netlist on a silicon wafer by arranging them together. The total area occupied by netlist in silicon wafer is 4 square units.

![image](https://user-images.githubusercontent.com/123365828/216042654-3192b2da-1777-41a2-b667-8b1f58e72bc3.png)

#### what is the core and die?
	
A 'core' is the section of the chip where the fundamental logic of the design is placed.

A 'Die', which is consist of core, is small semiconductor material specimen on which the fundamental circuits is fabricated.
	
![image](https://user-images.githubusercontent.com/123365828/216042866-c5c44507-1704-4b7d-a0aa-ad8b3ee6b1ea.png)

If we put the 'arranged netlist' in the core, then whole core is occupied by netlist. that means utilization of core is 100%.
	
![image](https://user-images.githubusercontent.com/123365828/216042964-1a66c1c0-87b4-4be2-905b-ae3561424d4b.png)

#### Utilization factor
	
it is defined as, 'the area occupied by netlist' devided by the 'total area of core'.

so, in above case, the utilization factor is 1.

practically, we don't go with 100% uti;ization. practically utilization factor is about 50%-60% to add other extra cells (like buffer) in the core after netlist is placed.

#### Aspect Ratio
it is defined as (height)/ (width). here in above example the aspect ratio is 1.

### Concept of pre-placed cells
	
### 2. Define locations of preplaced cells
	
to define the preplaced cells, let's take a combinational logic i.e., mux, multiplier, clock devider, etc. and assuming that the output of the circuit is huge and assuming that the circuit has some 100k gates. so let devides in two blocks of 50k gates.
	
![image](https://user-images.githubusercontent.com/123365828/216043293-4b95ca5f-12d3-42b3-82ae-d30b29f1520c.png)

This both blocks are implimented separatly. Now, extending the input and output pins from the both blocks. now, let's detached these box. we will black box the boxes and detached them. After black boxing, the upper portion is invisible from the top or invisible to the one , who is looking into the main netlist. now we make separate them out.
	
![image](https://user-images.githubusercontent.com/123365828/216043376-3d720304-ebca-48d7-84a4-1f3f4128d710.png)

This blocks are implemented in netlist once and then we can reuse it multiple time. Similarly, there are other modules or IPs also readily available.i.e.,memory, clock gating cell, comparater, mux. these all are part of the top level netlist.They recieve some signals, they perform some functions, they deliver the outputs but the functionality of the cell is implemented only once. That what we called as preplaced cells.

The arrangement of these ip's in a chip is refferd as floorplanning.These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "preplacement cells".These cells are place din such fashion that, the placement and routing tool not touch the location of the cell.

![image](https://user-images.githubusercontent.com/123365828/216043540-52e9daa3-12b6-4685-b06f-3dc379908eca.png)

Let consider memory as a preplaced cells as a block 'a', block 'b' and block 'c'. Memory of any device is implemented once and reused multiple times. these preplaced cells are located as per the design bacckground. the location of the cells are never touched.

### De-coupling capacitors
	
### 3. surround pre-placed cells with Decoupling capacitors
	
Let consider some circuit, which is part of the blocks which were defined above. When some gate (let consider AND gate) switched from 0 to 1 ot 1 to 0, considered amount of the switching current required because of small capacitance is available there. this capacitor should be completely charged to represent logic 1 and completly discharged to represent logic 0. the required amount of the charge is suplied from the Vdd and absorb from the Vss. during supplying the current, wire has some drop of voltage due to resistence and inductunce of the physical wire.
	
![image](https://user-images.githubusercontent.com/123365828/216043718-3741ad3a-bcd8-4ea1-9463-2c484a9b188d.png)

So, due to this if ideal logic 1 = 1 volt then here practically it can be less then 1 volt i.e., 0.97 volts (Vdd'). So, for any signal to be considered as Logic '0' and '1' in the NM low and NM high range. It is danger case.
	
![image](https://user-images.githubusercontent.com/123365828/216043804-4ba5a5be-612a-42f1-a269-fa94d4323590.png)

To solve this problem,, we have to put De-coupling capacitor in parallel with the circuit. Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replacenish the charge into Cd.
	
![image](https://user-images.githubusercontent.com/123365828/216043896-e4c05526-6500-4fe1-9be2-620eb978b9c7.png)

![image](https://user-images.githubusercontent.com/123365828/216043931-00a45030-f95e-47ff-a2f9-f511c5041c00.png)

### Power Planning
### 4. How to do power planning?
	
Up to here, we have taken care of local communication. Now let consider that local circuitory as a black box and it can be repeat multiple times and power supply is also connected to all of them as shown below,
	
![image](https://user-images.githubusercontent.com/123365828/216044097-3d790c2f-b6ff-4d23-b440-8946c145bcf7.png)

Now 16 bit bus has to retain the same signal from driver to the load. so it should get the sufficient power from the supply. But at this bus, there is no de-coupling capacitor is available because it is not physible to put capacitor at all over the place. now, power supply is far away from the bus, that is why some drop between them will definalty occurs.

Let consider this 16 bit bus connected to inverter. So, all capacitor are initially charged will get discharged and vice-versa due to inverter.

![image](https://user-images.githubusercontent.com/123365828/216044173-5c57b80e-ab08-4e65-8c9d-460706aaa340.png)

But the problem is occurs due to all capacitor is connected to the single ground. This will cause a bump in 'ground' tap point during discharging.
	
![image](https://user-images.githubusercontent.com/123365828/216044272-b6e8aeb7-9d8c-48fa-b883-258b5c2a86b0.png)

Same problem will occurse during the charging time also. at that time lowering of voltage occurse at 'Vdd' tap point.
	
![image](https://user-images.githubusercontent.com/123365828/216044393-aa06eed9-8d33-4dbb-ad1e-20f236887d12.png)

The solution of the problem is use multiple power supply. So, every block will take charge from neartest power supply and similarly dump the charge to the nearer ground. this type of power supply is called mesh.
	
![image](https://user-images.githubusercontent.com/123365828/216044520-ab7a9ed1-e95d-4018-b974-c7302e7fb437.png)

And the power planning is shown below,
	
![image](https://user-images.githubusercontent.com/123365828/216044601-ee4966d9-ba2f-429d-998d-7aebc923b933.png)

### Pin placement and Logic floor planning considerations

### Pin Placement
	
Lets take below designs for example that needs to implemented.
	
![image](https://user-images.githubusercontent.com/123365828/216044795-cf80c85b-4e68-4361-ade1-0ca5ce3aea71.png)

Both the sections has different inputs like Din1 and Din2 and operated from different clocks like clock 1 and clock 2. along with that we have preplaced cells as well. So, basically now we have 4 input ports and 3 output ports.

let's have one more design that needs to be implemented. this types of circuits are very much helpful to understand the timing analysis of inter clocks.

now complete design becomes like given below which has 6 input ports and 5 output ports. it is called the 'Netlist'.
	
![image](https://user-images.githubusercontent.com/123365828/216044988-26a7d492-7658-4d78-9eda-46acace5ce10.png)

Let's put this netlist in the core which we have designed before and let's try to fill this empty area between core and die with the pin information. The frontend team who decides the netlist input and output and the backend team who done the pin placements. So according to the pin placements, we have to locate the preplaced blocks nearer to the inputs of the preplaced blocks.
	
![image](https://user-images.githubusercontent.com/123365828/216045119-ce73cd77-6dda-4bde-a29f-c7e953945b05.png)

Here one thing that we noticed is that clock-in and clock-out pins are bigger in size as compared to input and output pins. reason behind this is that, input clocks are conntinuously provides the signal to the every elements of the chip and output clock should out the signal as fast as possible. So, we need least resistance path for the clocks inputs and clocks outputs. So, bigger the size, lower the resistance.

One more thing is need to take care about is that, this pin placement area is blocked for routing and cell placements. so we nned to do logical cell placement blockage. this blockage is shoown in above image in between pins.

So, floor plan is ready for Placement and Routing step.

### Steps to run floorplan using OpenLANE
	
Before run the floorplanning, we required some switches for the floorplanning. these we can get from the configuration from openlane.

![Capture1](https://user-images.githubusercontent.com/123365828/216514763-a47e9d36-aa45-4c65-b1b2-d62917d4424f.PNG)

Here we can see that the core utilization ratio is 50% (bydefault) and aspect ratio is 1 (bydefault). similarly other information is also given. But it is not neccessory to take these values. we need to change these value as per the given requirments also.

Here FP_PDN files are set the power distribution network. These switches are set in the floorplane stage bydefault in OpenLANE.

![Capture2](https://user-images.githubusercontent.com/123365828/216514841-bf5755fa-4989-469c-a9f4-1c546f19dcb9.PNG)

Here, (FP_IO MODE) 1, 0 means pin positioning is random but it is on equal distance.

In the OpenLANE lower priority is given to system default (floorplanning.tcl), the next priority is given to config.tcl and then priority is given to PDK varient.tcl (sky130A_sky130_fd_sc_hd_congig.tcl).

Now we see, with this settings how floorplan run.

### Reviewing floorplan files and steps to view floorplan

In the run folder, we can see the connfig.tcl file. this file contains all the configuration that are taken by the flow. if we open the config.tcl file, then we can see that which are the parameters are accepted in the current flow.

![Capture3](https://user-images.githubusercontent.com/123365828/216514951-40406d62-25e9-44d0-a17a-73e6f862fc61.PNG)

here we can see that, the core utilization is 35%, aspect ratio is 1 and core margin is taken as 0. while in default the core utilization is 50%. this is the issue. because this design is override the system. but it is the taken from PDK varient.tcl file. so priority vise it is true.

To watch how floorplane looks, we have to go in the results. in the result, one def( design exchange formate) file is available. if we open this file, we can see all information about die area (0 0) (660685 671405), unit distance in micron (1000). it means 1 micron means 1000 databased units. so 660685 and 671405 are databased units. and if we devide this by 1000 then we can get the dimensions of chips in micrometer.

![Capture4](https://user-images.githubusercontent.com/123365828/216515024-7994a76f-89e9-4ffa-a546-873c4389f8ab.PNG)
	
so, the width of chip is 660.685 micrometer and height of the chip is 671.405 micrometer.

To see the actual layout after the flow, we have to open the magic file by adding the command magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def

And then after pressing the enter, Magic file will open. here we can see the layout.
	
![Capture5](https://user-images.githubusercontent.com/123365828/216517453-5820797b-db6e-4979-971c-acc55b1e29d8.PNG)

### Reviewing floorplan layot with magic.
	
In the layout we can see that, input output pins are at equal distance.
	
![Capture6](https://user-images.githubusercontent.com/123365828/216517574-d9c1daad-523d-464f-b000-ec90974502a5.PNG)

after selecting (To select object, first click on the object and then press 's' from keyboard. the object will hight lited. to zoom in the object, click on the object and then press 'z' and for zoom out press 'sft+z') one input pin, if we want to check the location or to know at on which layer it is available, we have to open tkcon window and type "what". it will shows all the details about that perticular pin.
	
![Capture8](https://user-images.githubusercontent.com/123365828/216517772-ad8babd4-0ceb-49f9-9459-8332b1a6c582.PNG)
	
![Capture9](https://user-images.githubusercontent.com/123365828/216517893-37f7b134-e304-4936-92b7-29379c7fb8e0.PNG)

so, it show that the pin is in the metal 3.similarly doing for the vertical pins, we find that this pin is at metal 2.
	
![Capture10](https://user-images.githubusercontent.com/123365828/216517982-827322a1-7185-4829-85d0-99daf24e1e8d.PNG)
	
Along with the side rows,the Decap cells are arranged at the border of the side rows.
	
![image](https://user-images.githubusercontent.com/123365828/216518053-c1e58e93-4026-441e-bf6f-8d7d360dbcad.png)

Another cells also placed here, which is a tap cells. these cells are meant to avoide the latch-up problems in the CMOS devices. it connect N-well to the Vdd and substrate to the Ground. these tap cells are placed at diagonally equal distance.
	
![image](https://user-images.githubusercontent.com/123365828/216518153-a0c5bb17-55c5-4333-9503-e204528a5599.png)

In the floorplane, standerd cells are not placed but here standerd cells are available in the left side of the floorplan. we can see few boxes are there.
	
![image](https://user-images.githubusercontent.com/123365828/216518222-9895ab3f-1b8a-4a35-a7db-ff926316c9f8.png)

here we can see that first standerd cells is for buffer 1. similarly other cells are for buffer 2, AND gate etc.

### Library building and Placement
	
#### Netlist binding and initial place design
	
#### 1. bind netlist with physical cells
	
Taking netlist as what we taken before,
	
![image](https://user-images.githubusercontent.com/123365828/216518415-aa919340-cba3-4840-8fc8-983980550c73.png)
	
Here, we can see that every gate or flip-flops have a shape to understand the functionality of the element. But practically, this cells are square or rectangular boxes which has internally some logic to perform. So, here we are taking all the elements from netlist and giving them a perfact height and width with perticular dimention. These all cells together are called 'self'. And this self are stored in the lybrary. Library have all the innformation about all the blocks, like height, width , time delay, conditional innformation, etc. library also have a option for the similar cells (with same functionality) like this with different height and width. According to our space available at floorplanning we can choose out of them.

![image](https://user-images.githubusercontent.com/123365828/216518469-8b790cc1-1c4d-4c98-8e50-29f70736e1e6.png)

After giving size and shape to each and every box, next step is to take the boxes or element from library and placed in the floorplan. This is called placements of the cells.According to the design of the netlist, we have to put physical blocks in the floorplan which we have design before.Put all the blocks according to the input and output of that perticular blocks.
	
![image](https://user-images.githubusercontent.com/123365828/216518529-c041012b-24fb-4267-a6bb-2ab285a3f6fd.png)

up to here we have done stage one and stage two placement. Now we will going for stage 3 and 4. here we have to place FF1 of stage 3 nearer to the Din3 and FF2 of stage 3 nearer to the Dout3. But Din3 and Dout3 are at somme distance from eachother. same thing is there for FF1 and FF2 of stage 4. Let's we placed these all element in such manner that all elements are closed to it's input and output pins.

![image](https://user-images.githubusercontent.com/123365828/216518597-166a82b4-f532-48fd-9602-26faba891cd3.png)

But, the distance of FF1 of Stage 4 and Din4 is still far them others. By optimizing the placement, we can solve this problem.

### Optimize placement using estimated wire lenght and capacitance
	
#### 2. Optimize Placement
	
As we seen that the distance from Din2 to FF1 of stage 2 is higher. so if we connect the wire between them then resistance and capacitance of the wire comes in to the picture. and due to this the signal integrity can not maintain.

To maintain the integrity of the signal out from Din2 to FF1, we have to put some repeaters like buffers on between Din2 and FF1. But it will cause of loss of area.

In the stage 1, there is no need of any repeater to transmit the signal. But in stage 2, due to high distance, the lenth of wire is high and signal is not transmitted in perticular range. so we required repeater.
	
![image](https://user-images.githubusercontent.com/123365828/216518707-629eed91-3431-4238-89e0-ba8c1e555dc8.png)

#### Final Optimization
	
Similar as stage 2, in Stage 3 also we required the buffer between gate 2 and FF2.

![image](https://user-images.githubusercontent.com/123365828/216518835-46945352-5fcf-4caa-bce3-febebac4651c.png)
	
![image](https://user-images.githubusercontent.com/123365828/216518852-2aa63d56-50e2-4727-9b7c-670dbf8172f3.png)
	
Stage 4 is bit tricky as compared to other stages. image

Now we have to check that, what we have done is correct or not. For that we need to do Timing analysis by considering the ideal clocks and according to the data of analysis, we will understand that, the placement is correct or not.

### Need for libraries and characterization.
	
#### Library charactorization and modelling
	
In whole IC design, we have to go through synthesis, floor/power planning, placement, routing , STA. In all this steps one thing remain common, which is "Gates or Cells". That is where the library charactorization becomes very important.

#### Congestion aware placement using RePLAce
	
Right now we are not constrain about timing, but constrain about the congestion. so, we are making the congrstion is less.

The placement is donne in two stages. Global and detailed. In global placement, legalization is not happened but after detailed placement legalization will be done.

When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires.

Now opening the Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below,
	
![Capture11](https://user-images.githubusercontent.com/123365828/216519075-9f473ab8-9fdc-46fb-9787-331e56b19153.PNG)

If we zooom into this, we find the buffers, gates, flip flops in this.
	
![Capture12](https://user-images.githubusercontent.com/123365828/216519139-f73f16eb-81b8-463d-8b08-d7a085089b54.PNG)

### Cell design and characterization flows
	
#### Inputs for cell design flow
#### Cell design flow
	
As we know that standerd cells are placed in the library. And in the library many other cells are available which have same functionality but the size is different. As size is different the parameters like hVt, Ivt also different for each standerd cells.

If we take one of the standerd cells called inverter, it has their own design flow by this it can be understandable to EDA tool.

The cell design flow is devided into three part.

	- Inputs
	- Design steps
	- Outputs

![image](https://user-images.githubusercontent.com/123365828/216519767-1aaeebb7-10c3-4a97-b69a-3d8116a54938.png)

#### 1. inputs
	
inputs required for cell design is PDKs, DRC and LVS rules SPICE models, library and user defined specs.

### Circuit design steps
	
#### 2. design steps
	
steps are circuit design, layout design, characterization

### 3. Outputs
	
The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).

### Layout design steps
	
	- implement function
	
![image](https://user-images.githubusercontent.com/123365828/216520109-cb52f448-5314-4f6f-b5fd-e2675d4ecc88.png)

	- Derive the NMOS and PMOS network graph
	
![image](https://user-images.githubusercontent.com/123365828/216520224-a0108409-7738-48fa-8a39-3bc0389c1d87.png)

	- Obtain the Euler's path
	
![image](https://user-images.githubusercontent.com/123365828/216520384-102e6d1b-34c1-45b2-9be6-404c6140dfb9.png)

	- Stick diagram
	
![image](https://user-images.githubusercontent.com/123365828/216520482-5443ecae-d9e4-4552-a715-7fc6e68f098b.png)

	- Convert stick diagram into proper layout
	
![image](https://user-images.githubusercontent.com/123365828/216520580-7421b90a-380b-4287-aabe-e376bab61e48.png)

After layout design, we have to ecxtract the layout and characterize it. In characterization step, we can get the information about Timing, Noice,power.libs and function.

### Characterization Flow
	
As of now, from the circuit design and layout design, we have final layout of buffer cell. where two buffers are connected in series with each other.
	
![image](https://user-images.githubusercontent.com/123365828/216521314-b80e9565-7317-47d5-8534-356744d0439d.png)

Now steps of flow is:

	- Read the model file
	- read the extracted spice netlist
	- reconize the behavior of buffer
	- Read the subcircuit of the inverter
	- Ateched the neccessory power source
	- Apply the stimulus
	- Provide the necessory of output capacitance
	- Provide the necessory simulatin command
	- This all steps we have to give in "GUNA" software. and this software will give the timing, noise, power.libs and functions.

## General Timing characterization parameters
	
### Timing threshold defination
	
![image](https://user-images.githubusercontent.com/123365828/216521550-d9aa020c-9afa-4d1f-88c6-2dd7fc81c53e.png)

Let we take the waveform from the output of the first buffer and it will be input of the second buffer and taking output of the second buffer also.
	
![image](https://user-images.githubusercontent.com/123365828/216521608-846c5be4-15be-4d7e-b0a1-159d0d42c91b.png)

	- slew_low_rise_thr
	
here low means nearer to the ground, and rise tresold means we want to measer the slope of the increasing graph. typical value of slew low rise thr is around 20-30%.
	
![image](https://user-images.githubusercontent.com/123365828/216521706-67a7c657-9e1a-4ae0-a4e7-0bdf5c7c2d9e.png)

	- slew_high_rise_thr
	
same as above, high means nearer to high value.
	
![image](https://user-images.githubusercontent.com/123365828/216521949-4153db2c-ae91-469e-9b3f-4a0bab516071.png)

whenever we want to calculate the slew, take the point at 20% from the low and take the point at 20% from the high. according to these point, take the time data and the time difference between them will helps to calculate the slew.

	- slew_low_fall_thr
	
![image](https://user-images.githubusercontent.com/123365828/216522219-3e967df6-583d-408a-9d6c-9e3e62cfb5ae.png)

	- slew_high_fall_thr
	
![image](https://user-images.githubusercontent.com/123365828/216522391-b0924b80-1d4c-4a35-b4b2-b4f708c174c5.png)

Now, taking the waveform of input stimulus which is input of the first buffer and with that taking output of the first buffer.

Similar as a slew, thresolds are for delay also available. for that same as slew, we have to take some rise and fall points from the waveforms. this tresolds are almost 50%.

	- in_rise_thr
	
![image](https://user-images.githubusercontent.com/123365828/216522832-4b42c74c-494c-4f8d-9d49-1569168180ca.png)
	
	- in_fall_thr

![image](https://user-images.githubusercontent.com/123365828/216522932-2b99b1f2-5eb4-4510-b717-ff5b8bd5655e.png)
	
	- out_rise_thr
	
![image](https://user-images.githubusercontent.com/123365828/216523052-a7f9479a-f8fb-4424-a797-a15213f44f9d.png)
	
	- out_fall_thr
	
So, according to rise and fall theresold we can find the rise delay and fall delay of the buffer.
	
![image](https://user-images.githubusercontent.com/123365828/216523181-5535074d-775b-4a8b-aedd-87309c3c3230.png)

### Propogation delay and Transition time
	
#### propogation delay
	
let's take the same setup for understand the propogation delay. Time delay = Time(out_thr)-time(in_thr).

Let's take waveform on which we can apply above formula.
	
![image](https://user-images.githubusercontent.com/123365828/216523311-88fcd0ee-62d9-4611-9785-1e16a64b5484.png)

In any case if thresold point move at the top, at that time we get negative delay because output comes before input. so reason behind the negative delay is the poor choice of the tresold points. which is not acceptable. so, choosing the thresold point is very important.
	
![image](https://user-images.githubusercontent.com/123365828/216523368-46263fb6-6789-4cef-8e0f-82cac12caa2a.png)

Taking another example where the wire delay is very high because of high distance between two elements. so, here by choosing correct thresold point then also we get the negative delay.
	
![image](https://user-images.githubusercontent.com/123365828/216523419-1078de73-4f16-42e2-99f7-5027033becef.png)

### Transition time
	
transition time = time(slew_high_rise_thr)- time(slew_low_rise_thr)

or

transition time = time(slew_high_fall_thr)- time(slew_low_fall_thr)

let's take waveform for understand the slew calculation.
	
![image](https://user-images.githubusercontent.com/123365828/216523595-24d027c9-8e14-4cb8-ac1d-a5ae4ea573ed.png)

# Day 9 -Design library cells using Magic Layout and ngspice characterization

## Labs for CMOS inverter ngspice simulations

### SPICE deck creation for CMOS innverter

### IO placer revison

Till now, we have done floor planning and run placement also. But if we want to change the floorplanning, for example, in our floor planning, pins are at equal distance and if we want to change it then we can also make it by "Set" command.

For that first we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.

Then check the changes in the pins location through magic -T.

![Capture2](https://user-images.githubusercontent.com/123365828/216750012-bab92cfb-55ea-4872-9dd7-bea998dd0d56.PNG)

So, here we can see that there are no pins in the upper half side. all pins are in the lower half of the core.

### SPICE deck creation for CMOS inverter

#### VTC- SPICE simulations.

Before entering into the simulation, we have to creat the spice deck. The spice deck is nothing but netlist. so, we have to creat the spice deck for CMOS.

	- Component connectivity

![image](https://user-images.githubusercontent.com/123365828/216750345-9392881a-7096-4fc2-a318-5cd70c0079e8.png)
	
	- Define the components values

![image](https://user-images.githubusercontent.com/123365828/216750362-44bfd592-bf3a-472c-87fb-bb12e7d5562e.png)

	- Identyfy the nodes
	
![image](https://user-images.githubusercontent.com/123365828/216750375-24e44789-dbc2-467d-a554-4a484e414ef0.png)

	- Name 'Nodes'
	
![image](https://user-images.githubusercontent.com/123365828/216750389-2f648c55-7d72-49ee-aa73-335e2b50481e.png)

Now, let's strat the writing the SPICE deck

![image](https://user-images.githubusercontent.com/123365828/216750401-e0d4466c-8a20-4b2e-8acf-89f11e30d8f7.png)

Here, in the syntex, It is like Name of the mosfet, drain , gate, substrait , source. so, the meaning of the syntex is that, name of the mosfet is M1, Drain is connected to OUT node, Gate is connected to IN node, Substrait is connected to Vdd and the Source is connected to Vdd node. PMOS says the type of mosfet and the Width and lenth of channel is defined. similarly, For M2, syntex were written.

### SPICE simulation lab for CMOS

Tile we discribe the connectivity information about CMOS inverter only. Now we have to discribe connectivity information about other components also like source, capacitor etc. so, lets look into the other components.

First we discribe the load capacitor and then about the Vdd and Vin.

![image](https://user-images.githubusercontent.com/123365828/216750423-91713018-1a72-40ce-ac02-d7949a28b5bf.png)

Now, we have to give simulation command. which is about swiping the Vin from 0 to 2.5 with the steps of 0.05. Because we want Vout while changing the Vin.

![image](https://user-images.githubusercontent.com/123365828/216750428-8b3751a4-a3f2-4a35-94c2-72c1d622ca73.png)

The final step is to discribe the Model file.Model file contains all the details about PMOS and NMOS. from this file only we get the information about PMOS and NMOS.

![image](https://user-images.githubusercontent.com/123365828/216750441-a89d05dd-ec7b-4c90-9f60-20644a18d2d8.png)

So, the total program is given below,

![image](https://user-images.githubusercontent.com/123365828/216750450-54d381af-9af8-417c-a360-b32cbce98dbc.png)

Now, doing the simulation and get the graph like this,

![image](https://user-images.githubusercontent.com/123365828/216750454-a97a05b8-1dda-4bde-a8fc-a6b3a9d39ee4.png)

Now, doing other simulation in which we change the PMOS width to 3 times of NMOS width. and after diong the simulation, we get the graph like this,

![image](https://user-images.githubusercontent.com/123365828/216750461-facf8b9a-7473-4b20-bf0c-1c5c1ca98e0f.png)

The difference between this two graph is that in the second graph the transfer charactoristic is lies in the ecxact middle of the graph where in the first graph it is lies left from the middle of the graph.

### Switching Thresold Vm

These both model of different width has their own application. By comparing this both waveform, we can see that the shape of the both waveform is same irrespective of the voltage level.

This thing tell us that when Vin is at low, output is at high and when Vin is at high, the output is at low. so the charactoristic is maintain at all kind of CMOS with different size of NMOS or PMOS. That is why CMOS logic is very widely used in the design of the gates.

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.

![image](https://user-images.githubusercontent.com/123365828/216750487-12623e59-a9d6-4cbf-92d8-505309132622.png)

In this figure, we can see that at Vm~0.9v, Vin=Vout. This point is very critical point for the CMOS because at this point there is chance that both PMOS and NMOS are turned on. If both are turned on then there is chances of leakage current(Means current flow direcly from power to ground).

By comparing this both the graph we can understang the concept of switching thresold voltage.

![image](https://user-images.githubusercontent.com/123365828/216750495-1da4c89a-1af5-4d83-a0e3-a7e67786e251.png)

![image](https://user-images.githubusercontent.com/123365828/216750505-f9f9c5da-8d27-48e4-bb57-778a04e28fd2.png)

## Lab steps to git clone vsdstdcelldesign

To get the clone, copy the clone address from reporetery and paste in openlane terminal after the command "git clone". this will create the folder called "vsdstdcelldesign" in openlane directory.

![Capture3](https://user-images.githubusercontent.com/123365828/216750536-255b0ec1-822f-46b4-93ee-5d92f22e3424.PNG)

now, if we open the openlane directory, we find the vsdstdcelldesing folder in the openlane directory.

![Capture4](https://user-images.githubusercontent.com/123365828/216750560-3c067834-3afa-47c7-b5ad-65832ada94b0.PNG)

Now if we goes in the vsdstdcelldesign folder and open it, we get the .mag file,libs file etc.

![Capture5](https://user-images.githubusercontent.com/123365828/216750586-87713941-2ef7-43d2-8967-5f3739c33dc7.PNG)

now, let's open the .mag file and see that which layers are used to build the inverter. But before opening the mag file, we need tech file. so we will copy this file from this given below address,

![image](https://user-images.githubusercontent.com/123365828/216750795-532dfc9a-6cf6-421f-9210-74f4d0f118d0.png)

And do copy by "cp" command to the location which is given below,

![image](https://user-images.githubusercontent.com/123365828/216750829-0e32d055-3d7a-4b7d-b9eb-9c0cb5e1ca51.png)

![Capture6](https://user-images.githubusercontent.com/123365828/216750937-10c07633-b997-4728-b0da-aa2aedd1a6cf.PNG)

Now, we can see that this file is copied in the vsdstdcelldesign folder.

![Capture7](https://user-images.githubusercontent.com/123365828/216750709-400c0838-d979-4a37-b1d8-64e7a1437ad2.PNG)

Now, here to see the layout in magic, we don't need to write the whole address because we copy the tech file here.

Now, appying the magic command like this,

![Capture8](https://user-images.githubusercontent.com/123365828/216750734-6024ecc5-8a53-4f08-a252-8ff4516f08d2.PNG)
	
## Inception of layout CMOS fabrication process
	
### create active region
	
#### 1. selecting a substrate
	
we select P-type silicon substrate with high resistivity(5~50 ohms) with moderate dopping and orientation is (100).

#### 2. creating active region for transister
	
First, we create the isolation layer by depositing the Sio2 layer (~40nm) on the substrate.
	
![image](https://user-images.githubusercontent.com/123365828/216751442-b8a3346b-61d1-49fa-9e11-d0257d2908e2.png)
	
Now, we are depisiting the Si3N4 layer (~80 nm) on the Sio2 layer.
	
![image](https://user-images.githubusercontent.com/123365828/216751459-355ee0c7-8b46-4315-8ca6-fc991ab48ab5.png)

Now we do patterning by using depositing photoresist and using Mask 1 through UV light.

![image](https://user-images.githubusercontent.com/123365828/216751475-131bff89-0407-4eac-87c1-f8492f9cb5a9.png)

Now, first we remove the mask and doing etching of Si3N4 layer on the exposed area.

![image](https://user-images.githubusercontent.com/123365828/216751492-b947f7e9-4ec7-48e7-a10e-8f70c1a44aa2.png)
	
Now, next step is to remove photoresist by chamical reaction, because now to Si3N4 layer itslef behaves like good protecting layer for Sio2 layer. now, if we do LOCOS (local oxidation of silicon) process, the exposed sio2 part will gown and bird break also form. This grown sio2 will provide the perfect isolation between two PMOS and NMOS.

![image](https://user-images.githubusercontent.com/123365828/216751502-9c1a627c-8014-4171-b0dd-f6f605febe4e.png)

Next step is to etchout the Si3N4 layer by hot phosphoric acid.

![image](https://user-images.githubusercontent.com/123365828/216751522-ba3b6477-6922-4599-9994-c7f10c0978ab.png)
	
### Formation of N-well and P-well
	
#### 3. N-well and P-well formation
	
we can not form P-well and N-well at a same time. we have to protect a region while forming one of the region by photoresist. And then using mask 2 and UV light, we will do patterning of photoresist to form P-well.
	
![image](https://user-images.githubusercontent.com/123365828/216751540-68c18a1c-f14b-4d15-8a5a-20211262f3b0.png)
	
Now, the area where we want to form the P-well is exposed. now we remove the mask and by applying the ion implantaton method (~200kev)to form P-well using Boron. But still it is P implant. After performing the high temparature anneling, it will become P-well.

![image](https://user-images.githubusercontent.com/123365828/216751550-c60bf4f3-6d2d-4844-9a5d-0dc575383569.png)

We wiil do a similar process to form N-well by using mask 3 and using Phosphorus ions.

![image](https://user-images.githubusercontent.com/123365828/216751556-941cfb75-94c1-44e9-bd2d-f0dacbf4ae25.png)

till now depth of wells are not define. so, by putting into the high temparature furnace (drive-in diffusion), we will define the depth of wells.

![image](https://user-images.githubusercontent.com/123365828/216751569-7d442b9c-2fb7-4299-ab06-0079c7187358.png)

### Formation of gate terminal
	
#### 4. Gate formation
	
Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only we can control the thresold voltage. doping concentration and oxide capacitance will control the thresold voltage.

so, first we are maintain the doping concentration here. for that we use mask 4 and again doing the ion implantation of boron ion at lower energy (~60kev).

![image](https://user-images.githubusercontent.com/123365828/216751587-c0285d6d-f9b5-4e2c-9a0c-2158420c4beb.png)

same process we will repeat for N-well also by using mask 5 and Arsenic ion.

![image](https://user-images.githubusercontent.com/123365828/216751595-6166854c-f101-42de-9dde-dd02c64ac107.png)

Next step is that we have to fix the oxide layer. but before that we have to remove the oxide layer because this layer is got dammeged because of the privious processes. so,first we remove the layer using HF solution and again re-grown the high quality oxide layer with same thickness.

The final step is the deposition of polysilicon layer over oxide layer with more impurities for low resistance gate terminal.Then etched out this polysilicon layer by using mask 6 and photoresist.
	
![image](https://user-images.githubusercontent.com/123365828/216751610-b1589de2-b657-483f-bbb9-4fd1ac429742.png)

After etching, remove the photoresist and gate terminal looks like,
	
![image](https://user-images.githubusercontent.com/123365828/216751637-663a60e3-96d6-4419-8c35-f467bacb4684.png)

### Lightly doped drain (LDD) formation
	
#### 5. LDD formation
	
Here, we actully want P+,P-,N doping profile in the PMOS and N+,N-,P doping profile for NMOS. Reason for that is

	- Hot electron effect
	- short channel effect
	
For the formation of LDD, we again do ion implantation in P-well by using mask 7 and here we use phosphoros as a ion for light doping.

![image](https://user-images.githubusercontent.com/123365828/216751673-3de25a3b-3202-4160-9d54-30f25f8c0a17.png)

Same process we will repeat for N-well. there we use mask 8 and BOron Ion.

![image](https://user-images.githubusercontent.com/123365828/216751679-3c23f1eb-17ed-48be-9c2e-4b8db4adf6fa.png)

Now, by creating the spacers, we can protect the actual structre remain constant of P-implantt and N-implant. For that we deposite a thick Sio2 or Si3N4 layer over the gate tereminal.
	
![image](https://user-images.githubusercontent.com/123365828/216751690-8abb4bec-1762-4532-86c1-7fe274f1badc.png)

Now, we do Plasma anisotropic etching. By that side-wall spacers are formed.

![image](https://user-images.githubusercontent.com/123365828/216751705-05ad8ece-5bcb-4f56-accc-a6a9c68c6f91.png)

### Source and drain formation
	
#### 6. source-drain formation
	
Next step is deposite the very thin screen oxide layer to avoid the effect of channeling.
	
![image](https://user-images.githubusercontent.com/123365828/216751745-f40484fe-5f79-48b3-8fa1-07b5da08ead9.png)
	
Now to form the drain and source, again we do the ion implantation of arsenic at 75kev to create the N+ implant by using mask 9 in the P-well to form PMOS.
	
![image](https://user-images.githubusercontent.com/123365828/216751754-77f84d4b-d0bc-4028-bc1c-ca336bf9f0b7.png)
		
Same process we will repeat for NMOS by using the mask 10 and boron ion in the N-well at 50kev to creat P- implant.
	
![image](https://user-images.githubusercontent.com/123365828/216751773-d82f2ebf-7fcc-41c7-b1ba-c06b8485f037.png)
	
Now we put this Half made CMOS into the high temparature (1000 degree)anneling. So P+ implant and N+ implant now become the source and drain.
	
![image](https://user-images.githubusercontent.com/123365828/216751781-c6bb06df-75f4-4b94-b2e9-cc17bdb879ef.png)
	
### Local interconnect formation
	
#### 7. steps tp form contacts and local interconnects
	
First step is remove the thin screen oxide layer by etching. Then deposite the titanium (Ti) using sputtering. here Ti is used because Ti has very low resistivity.
	
![image](https://user-images.githubusercontent.com/123365828/216751803-987f4438-0db8-451d-809b-bc196336cc18.png)
	
Next step is to create the reaction between Ti layer and source, gate, drain of CMOS. For that wafer is heated at about 650-700 degree temparature in N2 ambient for about 60 seconds. and after reaction, we can see the titanium siliside over the wafer. One more reaction is heppend there between Ti and N. and it results the TIN which is used for local communication.
	
![image](https://user-images.githubusercontent.com/123365828/216751812-9910d106-7209-4dd3-9b8b-c6affd76c9a8.png)
	
Now by using mask 11 and photoresist, we will etched out the TIN and make perticular contacts. TIN is etched out by using RCA cleaning.
	
![image](https://user-images.githubusercontent.com/123365828/216751822-c228c521-0894-437f-849e-886136850beb.png)
	
Now, local interconnects are formed after etching and removing the photoresist.
	
![image](https://user-images.githubusercontent.com/123365828/216751831-0b39fcd8-dd73-4400-a38d-7fd9c4d08c0f.png)
	
### Higher level metal formation
	
#### 8. Higher level metal formation
	
These steps are very semilar like previous steps. First thing that we are noticing is that the surface is non planner. it is not good to use this type of non planner serface for matel interconnects because of the problems regarding the metal disconinuty. so, we have to plannerize the surface by depositing the thick layer of sio2 with some impurity to make less resistive layer. and then we used CMP (chemical mechanical polishing) technique to plannerise the surface.

![image](https://user-images.githubusercontent.com/123365828/216751858-f4d7f54f-558f-4e1d-8de1-366056dc4c36.png)
	
Now using mask 12 and photorsist we etched the sio2 layer to diposite the metal in it.
	
![image](https://user-images.githubusercontent.com/123365828/216751870-41403c83-7eae-4d9b-966c-fb0f24c14182.png)
	
Now remove the photoresist and seposite the thin later of TIN (~10nm) over the wafer. Because TiN is act as very good adession layer for sio2 and also act as a barrier between bottom layer and top layer of metal interconnects.
	
![image](https://user-images.githubusercontent.com/123365828/216751876-d4352032-8f71-426b-b643-a23b14d913c3.png)
	
Next step is to deposite the blanket tungsten (W) layer over the wafer. and then do the CMP here to plannerize the surface.
	
![image](https://user-images.githubusercontent.com/123365828/216751885-32fba7a0-5881-462c-871a-0c0fb9347093.png)
	
This W is act as a contact holes and this holes needs to connect to the Higher metal layer. so we will deposite the Al (aluminium) layer.
	
![image](https://user-images.githubusercontent.com/123365828/216751892-2d96e2bf-3717-4ab9-bd0c-0cbf54a353b4.png)
	
Then by using the mask 13 and photoresist, we etched the W layer out to form the contact at perticular place by Plasma etching.
	
![image](https://user-images.githubusercontent.com/123365828/216751907-5489c943-f320-425d-bdc4-6e286c402761.png)
	
This is our first level of metal interconnets. now we again do the same process as above to deposite the second level of metal interconnect by using mask 14 for etched out the sio2 and using mask 15 for etched out Al leyer.
	
![image](https://user-images.githubusercontent.com/123365828/216751915-298e1938-b0fb-4cee-b1b1-459e719020f2.png)
	
The upper layer of Al is bit thicker as compared to lower layer of Al.Now, again deposite the layer of sio2 or si3N4 to protect the chip.
	
![image](https://user-images.githubusercontent.com/123365828/216751935-c8206ff3-e95e-4bb7-a0f4-06a4852329e7.png)
	
And finally our CMOS is looks like this after the fabrication.
	
![image](https://user-images.githubusercontent.com/123365828/216751944-38304d63-bcd5-4e92-adf0-e0a31fea8ab0.png)

## Lab introduction to Sky130 basic layer layout and LEF using inverter
	
![Capture8](https://user-images.githubusercontent.com/123365828/216751958-8199d300-6e53-4f3c-8972-e42dcf1240e2.PNG)

In sky130, every color is showing the different layer. here the firsst layer is for local interconnect shown by blue_perpel color, then second layer is metal 1 which is showm by light perple color, and the metal 2 is shown by pink color. N-well is showm by solide das line. green is N-diffusion region. and red is for polysilicon gate. similarly the brown color is for P-diffusion.

In tckon window, we can see that the selected area is NMOS and similarly we can chech PMOS also. and that is how we can check that the CMOS is working or not.
	
![Capture9](https://user-images.githubusercontent.com/123365828/216752034-75b2f6c3-67ea-4850-b130-b2af7625b236.PNG)

semilarly we check for the output terminal also.(by double pressing "S" to select the entire thing at output Y).
	
![image](https://user-images.githubusercontent.com/123365828/216752486-0b4cf6ef-e967-4a24-b90d-bbd75234b517.png)

so, we can see that "Y" is attached to locali in cell def sky130_inv.

we can check the source of the PMOS is connected to the ground or not. and similarly we can check it for NMOS also.
	
### Lab steps to create std cell layout and extract spice netlist
	
To extract the file from here, we have to write the command in tckon window. and the comand is "extract all".
Now let's go to this location from the terminal. it is exctracted.
	
![Capture11](https://user-images.githubusercontent.com/123365828/216752785-e658dbc5-f35d-4a62-bcfd-bed58f24f00c.PNG)

we will use this .ext file to create the spice file to be use with our ngspice tool. for that we have apply the comand "ext2spice cthresh 0 rthresh 0". this will not create anything new. now again we have to type "ext2spice" comand in tckon window.
	
![Capture12](https://user-images.githubusercontent.com/123365828/216752790-d06c0ad7-afb6-44e7-9d45-03c963493b07.PNG)

so, now we are checking the location and at there spice file has been created.
	
![Capture13](https://user-images.githubusercontent.com/123365828/216752812-0f6c99b1-37f6-4a4a-9097-8d130e3bd630.PNG)

let's see what inside the spice file by "vim sky130_inv.spice".
	
![Capture14](https://user-images.githubusercontent.com/123365828/216752834-bae35153-7c53-4601-a448-ec9ce1a5d134.PNG)

### Sky130 Tech File labs
	
#### Lab steps to create final SPICE dexk using sky130 tech.
	
![Capture14](https://user-images.githubusercontent.com/123365828/216752834-bae35153-7c53-4601-a448-ec9ce1a5d134.PNG)
	
here, we can see the all details about the connectivity of the NMOS and PMOS and about the power supply also.

X0 is NMOS and X1 is PMOS and both's connectivity is shown as GATE DRAIN SUBSTATE SOURCE.

But here the scale is 10000 um. but in Magic simulation, it is 0.01.
	
![Capture15](https://user-images.githubusercontent.com/123365828/216752871-07dc2ac2-b490-491f-8913-fc3b626ba50f.PNG)

SO, we are going to change the dimension here in the terminal. so any measurement will be in this scale of 0.01u. i.e., width=37*0.01u.

Now we have to include the PMOS and NMOS lib files. it is inside the libs folder in the vsdstdcellsdesign folder.
	
![Capture16](https://user-images.githubusercontent.com/123365828/216752894-8256a133-6a05-4ca8-a017-386477e34fea.PNG)

so, now we include this file in the terminal by ".include ./libs/pshort.lib" and ".include ./libs/nshort.lib" comand.

And then set the supply voltage "VDD" to 3.3v by "VDD VPWR 0 3.3V" comand. and similarly set the value of VSS also.

Now, we need to specify the input files. by Va A VGND PULSE(0V 3.3V 0 0.1ns 2ns 4ns).

Also add the comand for the analysis like, ".tran 1n 20n", ".control" , "run",".endc",".end".
	
![Capture17](https://user-images.githubusercontent.com/123365828/216752971-5c66a515-c887-4c28-9b3c-c4c207615606.PNG)

after running this file we get output of ngspice like this,
	
![Capture19](https://user-images.githubusercontent.com/123365828/216752984-de0b028f-7d70-4286-b3f5-4e34df816d48.PNG)

Now, ploting the graph here by comand, "plot y vs time a".
	
![Capture20](https://user-images.githubusercontent.com/123365828/216753002-bb14471b-043e-4038-9c49-1345545a1e62.PNG)

### Lab steps to characterize inverter using sky130 model file
	
Here, we have to find value of 4 parameters.

#### rise time
	
it is time taken to the output waveform to 20% value to 80% value.



so, rise time= (4.00742-2.51611)e-09 = 1.4913 nsec.

#### fall time
	
it is the time take by output for transition from 80% to 20%.

![image](https://user-images.githubusercontent.com/123365828/216754165-fa10da84-9fbe-4ab7-b975-b4465727fff6.png)

so, fall time=0.08745 nsec.

#### propogation delay
	
it is the time difference between the 50% of input and 50% of the output.



so, propogation delay = 0.60185 nsec.

#### cell fall delay
	
it is time for output falling to 50% and input is rising to 50%.

![image](https://user-images.githubusercontent.com/123365828/216754816-6edab10f-ec41-49ed-9491-9fef0c601aba.png)

so, cell fall delay= 0.0778 nsec.

# Day 10 -Pre-layout timing analysis and importance of good clock tree
	
## Timing modelling using delay tables
	
### Lab steps to convert grid info to track info
	




	




	




