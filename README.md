# Exp-No: 01 - Write and simulate 4:1 Multiplexer using Verilog HDL and verify with testbench 


**Aim:** <br>
<br>
&emsp;&emsp;To design and simulate a 4:1 Mux using Verilog HDL and verify its functionality through a testbench using the Vivado 2023.1 simulation environment. 
<br>
**Apparatus Required:** <br>
<br>
&emsp;&emsp;Vivado 2023.1<br>
<br>
**Procedure:** <br>
<br>
1. Launch Vivado Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu.<br>
2. Create a New Project Click on "Create Project" from the Vivado Quick Start window. In the New Project Wizard: Project Name: Enter a name for the project (e.g., Mux4_to_1). Project Location: Select the folder where the project will be saved. Click Next. Project Type: Select RTL Project, then click Next. Add Sources: Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.). Make sure to check the box "Copy sources into project" to avoid any external file dependencies. Click Next. Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation).
Default Part Selection: You can choose a part based on the FPGA board you are using (if any). If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7). Click Next, then Finish.<br>
3. Add Verilog Source Files In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier. Add the Verilog files (mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.) and the testbench (mux4_to_1_tb.v).<br>
4. Check Syntax Expand the "Flow Navigator" on the left side of the Vivado interface. Under "Synthesis", click "Run Synthesis". Vivado will check your design for syntax errors. If any errors or warnings appear, correct them in the respective Verilog files and re-run the synthesis.<br>
5. Simulate the Design In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation". Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench.<br>
6. View and Analyze Simulation Results The simulation waveform window will display the signals (S1, S0, A, B, C, D, Y_gate, Y_dataflow, Y_behavioral, Y_structural). Use the time markers to verify the correctness of the 4:1 MUX output for each set of inputs. You can zoom in/out or scroll through the simulation time using the waveform viewer controls.<br>
7. Adjust Simulation Time To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings".
Under "Simulation", modify the Run Time (e.g., set to 1000ns).<br>
8. Generate Simulation Report Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results". Save the report for reference in your lab records.<br>
9. Save and Document Results Save your project by clicking File → Save Project. Take screenshots of the waveform window and include them in your lab report to document your results. You can include the timing diagram from the simulation window showing the correct functionality of the 4:1 MUX across different select inputs and data inputs.<br>
10. Close the Simulation Once done, by going to Simulation → "Close Simulation<br>
<br>

Input/Output Signal Diagram:

<img width="601" height="295" alt="image" src="https://github.com/user-attachments/assets/b25d321e-78ee-45b7-89f4-8babca4087b9" />


RTL Code:

4:1 MUX BEHAVIOURAL IMPLEMENTATION 

```
 module mux4to1(i,s,y);
 input [3:0]i;
 input [1:0]s;
 output reg y;
 always @(i,s)
 begin
 if (s[1]==0&&s[0]==0)
 y=i[0];
 else if (s[1]==0&&s[0]==1)
 y=i[1];
else if (s[1]==1&&s[0]==0)
 y=i[2];
 else
 y=i[3];
 end
 endmodule
```

4:1 MUX DATAFLOW IMPLEMENTTION

```
module mux4to1(i, s, y);
 input [3:0]i;
 input [1:0]s;
 output y;
 assign y = (~s[1] & ~s[0] & i[0]) |
 (~s[1] & s[0] & i[1]) |
 ( s[1] & ~s[0] & i[2]) |
 ( s[1] & s[0] & i[3]);
 endmodule

```

4:1 MUX GATE LEVEL IMPLEMENTATION

```
 module mux4to1(i,s,y);
 input [3:0]i;
 input [1:0]s;
 output y;
 wire [4:1]w;
 and g1(w[1],i[0],~s[1],~s[0]);
 and g2(w[2],i[1],~s[1],s[0]);
 and g3(w[3],i[2],s[1],~s[0]);
 and g4(w[4],i[3],s[1],s[0]);
 or g5(y,w[1],w[2],w[3],w[4]);
 endmodule
```

4:1 MUX STRUCTURAL IMPLEMENTATION

```
module mux2to1(input a, input b, input s, output y);
 assign y =(~s & a)|(s & b);
 endmodule
module mux4to1(i, s, y);
 input [3:0] i;
 input [1:0] s;
 output y;
 wire w1, w2;
 mux2to1 m1(i[0], i[1],s[0], w1);
 mux2to1 m2(i[2], i[3],s[0], w2);
 mux2to1 m3(w1,w2, s[1], y);
 endmodule  
```

TestBench:

4:1 MUX BEHAVIOURAL TESTBENCH IMPLEMENTATION

```
module mux4to1_tb;
    reg [3:0] i;
    reg [1:0] s;
    wire y;

    mux4to1 dut(i, s, y);

    initial begin
        i = 4'b1001;
        s = 2'b00;
        #20;
        s = 2'b01;
        #20;
        s = 2'b10;
        #20;
        s = 2'b11;
        #20;
    end
endmodule
```

4:1 MUX DATAFLOW TESTBENCH IMPLEMENTATION

```
module mux4to1_tb;
    reg [3:0] i;
    reg [1:0] s;
    wire y;

    mux4to1 dut(i, s, y);

    initial begin
        i = 4'b0110;
        s = 2'b00;
        #20;
        s = 2'b01;
        #20;
        s = 2'b10;
        #20;
        s = 2'b11;
        #20;
    end
endmodule
```

4:1 MUX GATE LEVEL TESTBENCH IMPLEMENTATION

```
module mux4to1_tb;
    reg [3:0] i;
    reg [1:0] s;
    wire y;

    mux4to1 dut(i, s, y);

    initial begin
        i = 4'b1001;
        s = 2'b00;
        #10;
        s = 2'b01;
        #10;
        s = 2'b10;
        #10;
        s = 2'b11;
        #10;
    end
endmodule
```

4:1 MUX STRUCTURAL TESTBENCH IMPLEMENTATION

```
module mux4to1_tb;
    reg [3:0] i;
    reg [1:0] s;
    wire y;

    mux4to1 dut(i, s, y);

    initial begin
        i = 4'b1010;
        s = 2'b00;
        #20;
        s = 2'b01;
        #20;
        s = 2'b10;
        #20;
        s = 2'b11;
        #20;
    end
endmodule
```

Output waveform:

4:1 MUX BEHAVIOURAL OUTPUT

<img width="1608" height="812" alt="image" src="https://github.com/user-attachments/assets/8f2f99cb-bb48-414e-8a41-ac0d30135fda" />

4:1 MUX DATAFLOW OUTPUT

<img width="1347" height="783" alt="image" src="https://github.com/user-attachments/assets/b19ec08d-eb5c-4ecb-b2f8-df859afd9f2f" />

4:1 MUX GATE LEVEL OUTPUT 

<img width="1329" height="735" alt="image" src="https://github.com/user-attachments/assets/732e096c-34ea-4284-bb16-b67dd6d54ef3" />

4:1 MUX STRUCTURAL OUTPUT

<img width="1371" height="709" alt="image" src="https://github.com/user-attachments/assets/93da933f-12a9-4537-a8c7-79597396b180" />


Conclusion:

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural. The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.


  
