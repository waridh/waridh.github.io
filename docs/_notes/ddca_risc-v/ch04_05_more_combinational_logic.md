# 4.5 More Combinational Logic

Something that we did not look at is the `always_comb` in SystemVerilog. This command will evaluate all the inputs on the right side of a `<=` or a `=` changes. More information. It is good practice to put combinational logic inside of a `always_comb` instead of an `always @(*)` because it gives you more safe guards. SystemVerilog will give you a warning if you put non-combinational logic inside the `always_comb` block. It's also better to use blocking assignment `=` than the non-blocking `<=` when it comes to combinational logic. Here is an example of combinational logic in SystemVerilog:

```SystemVerilog
module inv(input logic [3:0] a,
          output logic [3:0] y);
always_comb
    y = ~a;
endmodule
```

So we can use the `always` statement to describe combinational logic as well, and now we are able to use two different assignments, `=` and `<=`. The difference here is that `=` is blocking, which means that statement will be executed in order, like a programming language would. This is preferred for combinational logic. The `<=` is a non-blocking assignments, and it means that all non-blocking assignments will just happen as soon as they can.

To get rid of some confusion, `assign` is a continuous assignment, which will change the output immediately on any changes in the input. `always` doesn't always work like that, unless you are working with `always_comb`, but it gives you better flexibility in designing blocks that are synchronized.

Here is another example for a full adder using always:

```SystemVerilog
module fulladder(
input logic a, b, cin,
output logic s, cout
);

logic p, g;

always_comb
begin
p = a ^ b; // blocking
g = a & b; // blocking
s = p ^ cin; // blocking
cout = g | (p & cin); // blocking
end
endmodule
```

## 4.5.1 Case Statements

Case statements must always appear inside an `always` statement, so this is where it starts becoming more beneficial to use `always` instead of `assign`.

Case statements work like `else if` in C, or `switch` in Python. It is used to simplify combinational logic so that there are less designer errors.

`case` statements implies combinational logic if all the input cases are defined, else it will imply sequential, and hold the previous value on undefined cases. Here is an example of a seven segment display done using `always` and `case`:

```SystemVerilog
module sevenseg(
input logic [3:0] data,
output logic [6:0] segments
);

always_comb
    case(data)
        // 
        0: segments = 7'b111_1110;
        1: segments = 7'b011_0000;
        2: segments = 7'b110_1101;
        3: segments = 7'b111_1001;
        4: segments = 7'b011_0011;
        5: segments = 7'b101_1011;
        6: segments = 7'b101_1111;
        7: segments = 7'b111_0000;
        8: segments = 7'b111_1111;
        9: segments = 7'b111_0011;
        default: segments = 7'b000_0000;
    endcase
endmodule
```

`case` reads the value of the data input and then sends an output based on that. `default` case acts like the general `else` statements in other programming languages that covers the rest of the other cases.

```SystemVerilog
module decoder(
input logic [2:0] a,
output logic [7:0] y
);

always_comb
case(a)
3'b000: y = 8'b0000_0001;
3'b001: y = 8'b0000_0010;
3'b010: y = 8'b0000_0100;
3'b011: y = 8'b0000_1000;
3'b100: y = 8'b0001_0000;
3'b101: y = 8'b0010_0000;
3'b110: y = 8'b0100_0000;
3'b111: y = 8'b1000_0000;
default: y = 8'bxxxx_xxxx; //In case of x or z input.
endcase
endmodule
```

This is another example of using case statements for gate implementation. Here, we are making a decoder.

## 4.5.2 If statements

`always` can also contain `if` statements with the complementary `else`. Here is an example:

```SystemVerilog
module priorityckt(
input logic [3:0] a,
output logic [3:0] y
);

always_comb
if (a[3]) y =4'b1000;
else if (a[2]) y = 4'b0100;
else if (a[1]) y = 4'b0010;
else if (a[0]) y = 4'b0001;
else y = 4'b0000;
endmodule
```

This is very similar to a case, but the difference here is that it has more control over the condition that will activate it? Well since priority circuit will return `TRUE` on the output that corresponds with the MSB that also has `TRUE` This can only be used in an `always`.

## 4.5.3 Truth Tables with Don't Cares

```SystemVerilog
module priority_casez(input logic [3:0] a,
                     output logic [3:0] y);
always_comb
casez(a)
4'b1???: y = 4'b1000;
4'b01??: y = 4'b0100;
4'b001?: y = 4'b0010;
4'b0001: y = 4'b0001;
default: y = 4'b0000;
endcase
endmodule
```

The case statement in SystemVerilog acts like a regular case statement, but it will also interpret `?` as a don't care.

## 4.5.4 Blocking and Nonblocking assignment

Already covered a little bit of it. Blocking will run the expressions sequentially, while a nonblocking assignment will compete. Here is a more detailed guideline on the matter:

### SystemVerilog

1. Use `always_ff @ (posedge clk)` and nonblocking assignments to model synchronous sequential logic.
2. Use continuous assignment for simple combinational logic.
3. Use `always_comb` on more complicated combinational logic where `always` would be helpful.
4. DO NOT MAKE ASSIGNMENTS TO THE SAME SIGNAL IN MORE THAN ONE `always` STATEMENT OR CONTINUOUS ASSIGNMENT STATEMENTS.

| [Previous](ch04_04_sequential_logic.md) | [Home]() | [Next](ch04_06_finite_state_machine.md) | 
| --------------------------------------- | -------- | --------------------------------------- |
