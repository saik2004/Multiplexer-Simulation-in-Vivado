# Exp No:1 Multiplexer Simulation in Vivado using verilog hdl

## Aim:
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## Apparatus Required:
Vivado 2023.1

## Procedure
1. Launch Vivado
Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu.
2. Create a New Project
Click on "Create Project" from the Vivado Quick Start window.
In the New Project Wizard:
Project Name: Enter a name for the project (e.g., Mux4_to_1).
Project Location: Select the folder where the project will be saved.
Click Next.
Project Type: Select RTL Project, then click Next.
Add Sources:
Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.).
Make sure to check the box "Copy sources into project" to avoid any external file dependencies.
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation).
Default Part Selection:
You can choose a part based on the FPGA board you are using (if any).
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7).
Click Next, then Finish.
3. Add Verilog Source Files
In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files (mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.) and the testbench (mux4_to_1_tb.v).
4. Check Syntax
Expand the "Flow Navigator" on the left side of the Vivado interface.
Under "Synthesis", click "Run Synthesis".
Vivado will check your design for syntax errors. If any errors or warnings appear, correct them in the respective Verilog files and re-run the synthesis.
5. Simulate the Design
In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation".
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench.
6. View and Analyze Simulation Results
The simulation waveform window will display the signals (S1, S0, A, B, C, D, Y_gate, Y_dataflow, Y_behavioral, Y_structural).
Use the time markers to verify the correctness of the 4:1 MUX output for each set of inputs.
You can zoom in/out or scroll through the simulation time using the waveform viewer controls.
7. Adjust Simulation Time
To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings".
Under "Simulation", modify the Run Time (e.g., set to 1000ns).
8. Generate Simulation Report
Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records.
9. Save and Document Results
Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results.
You can include the timing diagram from the simulation window showing the correct functionality of the 4:1 MUX across different select inputs and data inputs.
10. Close the Simulation
Once done, close the simulation by going to Simulation → "Close Simulation".

## Logic Diagram

![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

## Truth Table

![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

## Verilog Code

## 4:1 MUX Gate-Level Implementation

module mux_v( A, B, C, D, S0, S1, Y ); 
input A;
input B;
input C;
input D;
input S0;
input S1;
output Y;
wire not_S0, not_S1;
wire A_and, B_and, C_and, D_and;


not (not_S0, S0);
not (not_S1, S1);


and (A_and, A, not_S1, not_S0);
and (B_and, B, not_S1, S0);
and (C_and, C, S1, not_S0);
and (D_and, D, S1, S0);


or (Y, A_and, B_and, C_and, D_and);
endmodule

## Testbench Code:

module multi_tb;
    reg A, B, C, D;
    reg S0, S1;
    wire Y;

    // Instantiate the multiplexer
    mux_v u_mux_v(
        .A(A), 
        .B(B), 
        .C(C), 
        .D(D), 
        .S0(S0), 
        .S1(S1), 
        .Y(Y)
    );

    initial begin
        // Test case 1: Select A
        A = 1; B = 0; C = 0; D = 0; S0 = 0; S1 = 0;
        #10;

        // Test case 2: Select B
        A = 0; B = 1; C = 0; D = 0; S0 = 1; S1 = 0;
        #10;
        
        // Test case 3: Select C
        A = 0; B = 0; C = 1; D = 0; S0 = 0; S1 = 1;
        #10;

        // Test case 4: Select D
        A = 0; B = 0; C = 0; D = 1; S0 = 1; S1 = 1;
        #10;

        

        $finish;
    end

    // Monitor outputs
    initial begin
        $monitor("Time = %0t, A = %0d, B = %0d, C = %0d, D = %0d, S0 = %0d, S1 = %0d, Y = %0d",
                 $time, A, B, C, D, S0, S1, Y);
    end

endmodule

 ## Output
 ![Screenshot (15)](https://github.com/user-attachments/assets/fc6e4ec4-134c-472d-a5ae-d72549ed79c9)



## 4:1 MUX Data Flow Implementation

module mux_v(a,b,c,d,s,y);
input a;
input b;
input c;
input d;
input[1:0]s;
output y;



assign y = (s==2'b00)? a:
            (s==2'b01)? b :
            (s==2'b10)? c : d;

endmodule

## Testbench Code:

module multi_tb;
    reg a, b, c, d;
    reg [1:0] s;
    wire y;

    // Instantiate the multiplexer
    mux_v u_mux_v(
        .a(a), 
        .b(b), 
        .c(c), 
        .d(d), 
        .s(s), 
        .y(y)
    );

    initial begin
        // Test case 1: Select a
        a = 1; b = 0; c = 0; d = 0; s = 2'b00;
        #10;

        // Test case 2: Select b
        a = 0; b = 1; c = 0; d = 0; s = 2'b01;
        #10;

        // Test case 3: Select c
        a = 0; b = 0; c = 1; d = 0; s = 2'b10;
        #10;

        // Test case 4: Select d
        a = 0; b = 0; c = 0; d = 1; s = 2'b11;
        #10;

                $finish;
    end

    // Monitor outputs
    initial begin
        $monitor("Time = %0t, a = %0d, b = %0d, c = %0d, d = %0d, s = %0b, y = %0d",
                 $time, a, b, c, d, s, y);
    end

endmodule

## Output
![Screenshot (16)](https://github.com/user-attachments/assets/c9e37dc7-4435-466b-ab7e-1728245fa1f8)



## 4:1 MUX Behavioral Implementation

module mux_v(a,b,c,d,s,y);
input a;
input b;
input c;
input d;
input[1:0]s;
output reg y;

always@(*)begin

y <= (s==2'b00)? a:
     (s==2'b01)? b :
     (s==2'b10)? c : d;
end
endmodule

## Testbench Code:

module multi_tb;
    reg a, b, c, d;
    reg [1:0] s;
    wire y;

    // Instantiate the multiplexer
    mux_v u_mux_v(
        .a(a), 
        .b(b), 
        .c(c), 
        .d(d), 
        .s(s), 
        .y(y)
    );

    initial begin
        // Test case 1: Select input a
        a = 1; b = 0; c = 0; d = 0; s = 2'b00;
        #10;

        // Test case 2: Select input b
        a = 0; b = 1; c = 0; d = 0; s = 2'b01;
        #10;

        // Test case 3: Select input c
        a = 0; b = 0; c = 1; d = 0; s = 2'b10;
        #10;
        
        // Test case 4: Select input d
        a = 0; b = 0; c = 0; d = 1; s = 2'b11;
        #10;

        $finish;
    end

    // Monitor outputs
    initial begin
        $monitor("Time = %0t, a = %0d, b = %0d, c = %0d, d = %0d, s = %0b, y = %0d",
                 $time, a, b, c, d, s, y);
    end

endmodule

## Output
![Screenshot (14)](https://github.com/user-attachments/assets/23bbfe8d-377e-4b68-a0bb-e511eb9b85fb)


## 4:1 MUX Structural Implementation

module mux_v(
    a, b, c, d,  // Inputs
    s0, s1,      // Select lines
    y           // Output
);

    input a, b, c, d;
    input s0, s1;
    output y;

    wire not_s0, not_s1;
    wire a_and, b_and, c_and, d_and;

    // Invert select lines
    not u_not_s0(not_s0, s0);
    not u_not_s1(not_s1, s1);

    // AND gates for each input
    and u_a_and(a_and, a, not_s1, not_s0);
    and u_b_and(b_and, b, not_s1, s0);
    and u_c_and(c_and, c, s1, not_s0);
    and u_d_and(d_and, d, s1, s0);

    // OR gate to combine AND outputs
    or u_or(y, a_and, b_and, c_and, d_and);

endmodule

## Testbench Code:

module multi_tb;
    reg a, b, c, d;
    reg s0, s1;
    wire y;

    // Instantiate the multiplexer
    mux_v u_mux_v(
        .a(a), 
        .b(b), 
        .c(c), 
        .d(d), 
        .s0(s0), 
        .s1(s1), 
        .y(y)
    );

    initial begin
        // Test case 1: Select a
        a = 1; b = 0; c = 0; d = 0; s0 = 0; s1 = 0;
        #10;

        // Test case 2: Select b
        a = 0; b = 1; c = 0; d = 0; s0 = 1; s1 = 0;
        #10;

        // Test case 3: Select c
        a = 0; b = 0; c = 1; d = 0; s0 = 0; s1 = 1;
        #10;

        // Test case 4: Select d
        a = 0; b = 0; c = 0; d = 1; s0 = 1; s1 = 1;
        #10;
        
        $finish;
    end

    // Monitor outputs
    initial begin
        $monitor("Time = %0t, a = %0d, b = %0d, c = %0d, d = %0d, s0 = %0d, s1 = %0d, y = %0d",
                 $time, a, b, c, d, s0, s1, y);
    end

endmodule


## Output
![Screenshot (17)](https://github.com/user-attachments/assets/12a0f7fc-5a87-40bc-a917-15ffe0b0add2)



## Conclusion:

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural. The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.



  
