Aim
To design and simulate a Booth Multiplier using Verilog HDL and verify its functionality in Cadence nclaunch.

Tools Required
Cadence EDA Suite
Cadence nclaunch (for simulation)
Computer System
Minimum 4GB RAM and a multi-core processor
Theory
Booth’s multiplication algorithm is an efficient way to perform signed integer multiplication. It reduces the number of add/subtract operations and handles negative numbers efficiently.

Booth's Algorithm Steps:
Initialize the multiplier and multiplicand.
Examine the least significant bit (LSB) and previous bit:
If 01, add the multiplicand.
If 10, subtract the multiplicand.
If 00 or 11, do nothing.
Shift the result right after each step.
Repeat for n bits.
Simulation Procedure
Launch Cadence nclaunch and create a new Verilog project.
Write the Booth Multiplier code and compile it.
Apply test inputs using a Verilog testbench.
Run the simulation and observe the output waveforms.
Verify correctness against expected results.
Flow Chart
![image](https://github.com/user-attachments/assets/02ec643c-e357-45f5-a20f-78cef6d3f751)


Verilog Code for Booth Multiplier
module booth_multiplier(
// control signals
input clk,
input load,
input reset,
//inputs
input [3:0] M,
input [3:0] Q,
//outputs
output [7:0] P);

reg [3:0] A          = 4'b0;
reg Q_minus_one = 0;
reg [7:0] P           = 8'b0;
reg [3:0] Q_temp      = 4'b0;
reg [3:0] M_temp      = 4'b0;
reg [2:0] Count       = 3'd4;

always @ (posedge clk)
begin
    if (reset == 1)
    begin
        A          = 4'b0;           // reset values
        Q_minus_one = 0;
        P          = 8'b0;
        Q_temp     = 4'b0;
        M_temp     = 4'b0;
        Count      = 3'd4;
    end

    else if (load == 1)
    begin
        Q_temp     = Q;
        M_temp     = M;
    end

    else if ((Q_temp[0] == Q_minus_one) && (Count > 3'd0))
    begin
        Q_minus_one = Q_temp[0];
        Q_temp      = {A[0], Q_temp[3:1]};  // right shift Q
        A           = {A[3], A[3:1]};       // right shift A
        Count       = Count - 1'b1;
    end

    else if ((Q_temp[0] == 0) && (Q_minus_one == 1) && (Count > 3'd0))
    begin
        A           = A + M_temp;
        Q_minus_one = Q_temp[0];
        Q_temp      = {A[0], Q_temp[3:1]};  // right shift Q
        A           = {A[3], A[3:1]};       // right shift A
        Count       = Count - 1'b1;
    end

    else if ((Q_temp[0] == 1) && (Q_minus_one == 0) && (Count > 3'd0))
    begin
        A           = A - M_temp;
        Q_minus_one = Q_temp[0];
        Q_temp      = {A[0], Q_temp[3:1]};  // right shift Q
        A           = {A[3], A[3:1]};       // right shift A
        Count       = Count - 1'b1;
    end

    else
    begin
        Count = 3'b0;
    end

    P = {A, Q_temp};
end

endmodule
Verilog Test bench Code for Booth Multiplier
module booth_multiplier_tb;

// Inputs
reg clk;
reg load;
reg reset;
reg [3:0] M;
reg [3:0] Q;

// Output
wire [7:0] P;

// Instantiate the Design Under Test (DUT)
booth_multiplier dut (
    .clk(clk),
    .load(load),
    .reset(reset),
    .M(M),
    .Q(Q),
    .P(P)
);

// Clock Generation
always #10 clk = ~clk;

initial begin
    // Initialize Inputs
    clk = 0;
    load = 0;
    reset = 1'b1;         // Assert reset
    M = 4'b0111;          // M = 7
    Q = 4'b1011;          // Q = -5

    #20;
    load = 1;             // Load M and Q
    reset = 1'b0;         // Deassert reset

    #20;
    load = 0;             // Start processing

    #150;
    $finish;
end

endmodule

Truth Table for Booth Multiplier (Example)
![image](https://github.com/user-attachments/assets/97e9b814-af41-4689-a01a-1525cb7f90c7)


Nclaunch Work Library Window
![image](https://github.com/user-attachments/assets/72b92a17-0661-463e-933b-2e214352bb2b)


Simulation Results
![image](https://github.com/user-attachments/assets/555c2e22-e612-4164-8ac5-4bd1c4619012)


Results
Successfully designed and simulated a Booth Multiplier in Verilog. Performed signed multiplication efficiently. Verified correctness using Cadence nclaunch.
