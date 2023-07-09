# 4.4 Sequential Logic

HDL synthesizers recognize certain idioms and turn them into specific sequential circuits. We are here to use the proper idioms to describe the registers and latches.

## 4.4.1 Registers

Commercial systems all use registers now. They are usually built with a positive edge-triggered D flip-flop. Here is an example implementation of a d-ff in SystemVerilog:

```SystemVerilog
module flop(input logic clk,
           input logic [3:0] d,
           output logic [3:0] q);

    always_ff @(posedge clk)
    q <= d;
endmodule
```

So in general, a SystemVerilog `always` statement is written in the form:

```SystemVerilog
always @(sensitivity list)
    statement;
```

This statement will only occur when the event specified in the sensitivity list occurs. This is like a while loop that will only continue on a positive edge on the clock.

In our example `q <= d`, `q` copies the state of `d` on a positive edge of the `clk`, and otherwise will remember the old state of `q`. A sensitivity list is also called a stimulus list.

`<=` is called a nonblocking assignment. It's like a regular `=` but there are some nuance that makes it usable on sequential logic.

The `always` statement can be used to imply `flip-flops`, `latches`, or combinational logic, depending on the sensitivity list and statement. Sometimes, all this flexibility makes it easy to produce wrong hardware. This is why SystemVerilog introduced `always_ff`, `always_latch`, `always_comb` to basically increase the type checking. `always_ff` acts like a regular `always` but it will always implicitly imply a `flip-flop`. It will produce a warning if anything else is implied.

## 4.4.2 Resettable Registers

On system bootup, or initial state, the output of a ff will be unknown. This is denoted by `x` in SystemVerilog. A *resettable register* is a way to put your system in a known state on bootup. This can be done either asynchronously, or synchronously. Doing so asynchronously will make the process occur immediately, while a synchronous implementation will follow the next clk edge that is specified in the implementation.

### SystemVerilog Implementation

```SystemVerilog
module flopr(input logic clk,
            input logic reset,
            input logic [3:0] d,
            output logic [3:0] q);

// Asynchronous reset
// We now have a new input, being the reset. When the reset reset occurs, then the
// system will set the output of this flop to 0. Else, it's just a regular flop. So
// the always_ff with trigger either on the clk or the reset. This means it could
// ignore the clk if a reset comes in. In fact, you can replace the comma with the
// word or.
always_ff @(posedge clk, posedge reset)
    if (reset) q <= 4'b0;
    else q <= d;
endmodule

module flopr (input logic clk,
             input logic reset,
             input logic [3:0] d,
             output logic [3:0] q);

// Synchronous reset
// The difference here is that reset is no longer being used to trigger the action.
always_ff @(posedge clk)
    if (reset) q <= 4'b0;
    else q <= d;
endmodule
```

`always` with multiple items in the sensitivity list can have those items be separated by either a comma `,` or an or `or`.

## 4.4.3 Enable Registers

An enable register will only respond to the clock when the enable is active. You can stack reset on top of that, which would mean that the flop will only retain its value if both enable and reset are FALSE.

### Resettable and Enable Register Example

```SystemVerilog
module flopenr(
    input logic clk,
    input logic reset,
    input logic en,
    input logic [3:0] d,
    output logic [3:0] q
);
// The name means flip-flop enable reset.

always_ff @(posedge clk, posedge reset)
if (reset) q <= 4'b0;
else if (en) q <= d;
endmodule
```

## 4.4.4 Multiple Registers

In SystemVerilog you can use a single `always` to describe multiple pieces of hardware. For example, let's look at a synchronizer.

### Synchronizer: SystemVerilog

```SystemVerilog
module sync(input logic clk,
           input logic d,
           output logic q);

logic n1;

always_ff @(posedge clk)
    begin
        n1 <= d; // Non-blocking
        q <= n1; // Non-blocking
    end
endmodule
```

So now we have a new construct, being the `begin` and `end`. This needed to be done because there are multiple statements showing up inside the `always`.  It is like using `{}` in C.

## 4.4.5 Latches

It is good practice to avoid latches, but it is good to know how to implement them so that you can track them down and fix it if you have an unintended latch. Cases where latches are the best idea are not very common, and it is best to avoid dealing with that.

```SystemVerilog
module latch(input logic clk,
            input logic [3:0] d,
            output logic [3:0] q);
always_latch
 if (clk) q <= d;
endmodule
```

| [Previous](ch04_03_structural_modeling.md) | [Home]() | [Next](ch04_05_more_combinational_logic.md) | 
| ------------------------------------------ | -------- | ------------------------------------------- |
