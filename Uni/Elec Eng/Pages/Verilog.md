Used for custom chips, board level design, FPGAs, gate arrays.
<iframe src="https://www.geeksforgeeks.org/electronics-engineering/getting-started-with-verilog/" width = 100% height= 500px></iframe>
## Syntax
### Module
- A module is a block of hardware with inputs and outputs (try keep modules as small as possible)
- Always begins with 'module' and ends with 'endmodule'.

```
module test_1(out1, out2, in1, in2);
	input in1, in2;
	output reg out1, out2;
	
	always @(in1,in2)
		out1 = in1 & in2;
	always @(in1, in2)
		out2 = in1 | in2;
		
endmodule
```

### Always Block
- Triggers simulator based on inputs in 'sensitivity list'.
- Creates a block of hardware

```
always@(in1, in2)
	out1 = in1 & in2
```

#### Always Block Alternative
```
assign out1 = in1 & in2;
```

### Wires vs Reg
If something is in an always block, it HAS to be type reg (not a register)
If its not, it is a wire

### Combination Logic Circuits
```
wire[2:0] temp;

assing temp = {in1, in2, in3};

always@(temp) begin
	case (count)
		3'b000 : begin ...
		3'b001 : begin ...
		3'b011 : begin ...
		default : begin...
	endcase
end

```
- This is a truth table equivalent
- You define you output after begin ... for your 3'bits xxx

### Datapaths

```
module simple_datapath (out1, out2, out3, in1, in2, in3, in4, in5, in6);
	input in1, in2, in3, in4, in5, in6;
	output out1, out2, out3;
	
	wire[5:0] temp;
	
	simple_comb_function comb1(.a(in1),.b(in2),.x(temp[0]),.y(temp[1]));
	simple_comb_function comb2(.a(in3),.b(in4),.x(temp[2]),.y(temp[3]));
	simple_comb_function comb3(.a(in5),.b(in6),.x(temp[4]),.y(temp[5]));
	
	simple_comb_function4 comb4(.a(temp[0)),.b(temp[1]),.c(temp[2]),
	.d(temp[3)),.e(temp[4]),.f(temp[5]), 
	
.x(out1),.y(out2),.z(out3));
endmodule
```

![[Pasted image 20260224113325.png]]

### 
