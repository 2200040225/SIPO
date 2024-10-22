# SIPO

Design code:

module sipo_shift_register (
    input clk,        
    input reset,     
    input serial_in,  
    output reg [3:0] parallel_out 
);
    always @(posedge clk or posedge reset)
    begin
        if (reset)
        begin
            parallel_out <= 4'b0000;  
        end 
        else 
        begin
            parallel_out <= {parallel_out[2:0], serial_in};
        end
    end
endmodule

Test bench:

module tb_sipo_shift_register;
    reg clk;
    reg reset;
    reg serial_in;
    wire [3:0] parallel_out;
    sipo_shift_register uut (
        .clk(clk),
        .reset(reset),
        .serial_in(serial_in),
        .parallel_out(parallel_out)
    );
    always #10 clk = ~clk;
    initial begin
        clk = 0;
        reset = 0;
        serial_in = 0;
        reset = 1;
        #20;
        reset = 0;
        serial_in = 1; #20;
        serial_in = 0; #20;
        serial_in = 1; #20;
        serial_in = 1; #20;
        #40;
        $finish;
    end
    initial begin
        $monitor("Time: %0d, serial_in = %b, parallel_out = %b", $time, serial_in, parallel_out);
    end
endmodule

