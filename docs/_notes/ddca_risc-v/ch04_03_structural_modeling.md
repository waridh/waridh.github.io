# 4.3 Structural Modeling

The previous section was describing behavioural modeling, where we were modeling the relationship between inputs and outputs. Now we are going to discuss structural modeling, which focuses on how complex modules can be made out of simpler modules. This is exceedingly similar to programming.

Here is a SystemVerilog example of how we can make a 4:1 multiplexer from two 2:1 multiplexers:

```SystemVerilog
module mux4(input logic [3:0] d0, d1, d2, d3,
            input logic [1:0] s,
            output logic [3:0] y);
logic [3:0] low, high;

mux2 lowmux(d0, d1, s[0], low);
mux2 highmux(d2, d3, s[0], high);
mux2 finalmux(low, high, s[1], y);
endmodule

```

So there are three `mux2` instances here, `lowmux`, `highmux`, and `finalmux`. The `mux2` module needs to be defined elsewhere in the SystemVerilog code.

![](../../../assets/Pasted%20image%2020230703194233.png)

This is the resulting schematics. The thing to note here is that we are actually working with four four bit busses, and not just four 1 bit signals. Each occurrence of a mux2 here is called an instance. Think objects and classes. Modules are like a hybrid between classes and functions.

## Reimplementation of `mux2`

```SystemVerilog

module mux2 (input logic [3:0] d0, d1,
            input logic s,
            output tri [3:0] y);

    tristate(d0, ~s, y);
    tristate(d1, s, y);
endmodule
```

So the expression of `~s` are permitted in the port list for an instance. Arbitrarily complicated expressions are legal, but discouraged because they make the code difficult to read.

Modules can also access parts of a bus. Here, we have an example of how to create an 8-bit-wide 2:1 multiplexer using two 4-bit 2:1 multiplexers.

```SystemVerilog
module mux2_8(input logic [7:0] d0, d1,
             input logic s,
             output logic [7:0] y);

mux2 lsbmux(d0[3:0], d1[3:0], s, y[3:0]);
mux2 msbmux(d0[7:4], d1[7:4], s, y[7:4]);
endmodule
```

Just like functional programming design and other paradigms, it is best to build complex systems hierarchically. We start by instantiating major components and building down recursively until the pieces are simple enough to describe recursively. It is good style to avoid mixing structural and behavioural description in the same module.

| [Previous](ch04_02_combinational_logic.md) | [Home]() | [Next](ch04_04_sequential_logic.md) | 
| ------------------------------------------ | -------- | ----------------------------------- |
