# Background
## Develop Environment
Xilinx: Vivado, ISE
Altera: Quartus
## Develop Process
1. HDL Hardware Description Language： Verilog
2. RTL (Register Transfer Level) simulation
3. 芯片烧录与调试
# Module
```Verilog
// Port Declaration
module module_name (
		clk,
		rst_n,
		dout
	);
// Parameters
	parameter DATA_W = 8;
// Port Definition
	input clk;
	input rst_n;
	reg [DATA_W-1:0] dout;
	reg signal1;
// Functions
	always@(*)begin
		end
	assign;
endmodule
// Use Module
module_name instance_name(
	.clk   (iclk),
	.rst_n (irst_n ),
	,dout  (idout)
);
```
signal types:
wire: default
reg: modified in `always` block of the current module