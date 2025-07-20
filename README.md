# Ram-syn1
module single_port_ram (
    input [7:0] data,          // Input data
    input [5:0] addr,          // Address input
    input we,                  // Write enable
    input clk,                 // Clock input
    output reg [7:0] q        // Output data
);

   reg [7:0] ram [63:0];      // 64x8-bit RAM

  always @(posedge clk) begin
        if (we)
            ram[addr] <= data; // Write data to RAM
        else
            q <= ram[addr];   // Read data from RAM
    end

endmodule
 Testbench:



module tb_single_port_ram;

  reg [7:0] data;
    reg [5:0] addr;
    reg we;
    reg clk;
    wire [7:0] q;

   
   single_port_ram uut (
        .data(data),
        .addr(addr),
        .we(we),
        .clk(clk),
        .q(q)
    );

   
   always #5 clk = ~clk;

   initial begin
        
   clk = 0;
        we = 0;
        addr = 6'd0;
        data = 8'h00;

        
  $dumpfile("dump.vcd");
  $dumpvars(0, tb_single_port_ram);

        
   #10;
   we = 1;
   addr = 6'd10;
   data = 8'hA5;
    #10;
    we = 0;

        
  addr = 6'd10;
        #10;
        $display("Read data: %h", q);

   #10;
        we = 1;
        addr = 6'd20;
        data = 8'h3C;
        #10;
        we = 0;

        
   addr = 6'd20;
        #10;
        $display("Read data: %h", q);

        
  #10;
        $finish;
    end

endmodule
