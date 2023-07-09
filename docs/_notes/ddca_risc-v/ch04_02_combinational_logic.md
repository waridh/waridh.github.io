# 4.2 Combinational Logic

This chapter goes through some information about designing combinational logic using HDLs. The output of these circuits will only depend on the current input and have no memory. It's really about learning how to write behavioural models of combinational logic with HDLs.

## 4.2.1 Bitwise Operators

*Bitwise* operators act one a single-bit signal or multibit buses. For example, the `inv` module can be written in SystemVerilog as such:

```SystemVerilog
module inv(input logic [3:0] a,
          output logic [3:0] y);
    assign y=~a; // This makes the y output just the inversion of a
endmodule
```

SystemVerilog lets you choose either little-endian or big-endian. This is done via the order of the index that is put to choose the bus. `[3:0]` is little-endian, while `[0:3]` would be big-endian.

Choosing the endian of the bus has always been arbitrary, and it does not matter in the example circuit that was illustrated here. The book that I am reading is choosing to work with little-endian for the HDL designs that are going to be taught.

Here are some more examples with SystemVerilog:

```SystemVerilog
module gates(input logic [3:0] a, b,
            output logic [3:0] y1, y2, y3, y4, y5);
    /* five different two-input logic gates acting on 4-bit busses*/

    assign y1 = a & b; // AND
    assign y2 = a | b; // OR
    assign y3 = a ^ b; // XOR
    assign y4 = ~(a & b); //NAND
    assign y5 = ~(a | b); //NOR
endmodule
```

SystemVerilog has some operators built in to represent Boolean logic, but not for all gates, as some of them, we have to construct ourselves `&, |, ^` . When operators are combined with operands, it is called an expression: `a & b`. A statement includes the assignment to an output: `assign y1 = a & b`.

Something interesting in SystemVerilog are *continuous assignment statements*. Here is an inline example: `assign out = in1 op in2`. Basically, this syntax changes the state of out based on the inputs. This is the foundation to combinational logic.

## 4.2.2 Comments and White Spaces

This subsection teaches electrical engineers to comment code. I don't have that background, and so, know how to comment well.

## 4.2.3 Reduction Operators

Reduction operators imply a multi-input gate acting on a single bus.

## 4.2.4 Conditional Assignment

*Conditional assignment* select the output from among alternatives based on an input called *condition*. It's literally a conditional from programming.

Here is how you can write a 2:1 multiplexer using conditional assignment.

```SystemVerilog
module mux2(
    input logic [3:0] d0, d1,
    input logic s,
    output logic [3:0] y
);
assign y = s ? d1 : d0;  // Holy crap, this is just like Rust
endmodule
```

So the `?` operator chooses based on the first expression. When `s == 1`, then it will pick the second expression `d1`, else it will pick `d0`. At a higher level of abstraction, a multiplexer is just a bunch of conditionals.

Slightly unrelated, the `?` operator is called a ternary operator because it takes three inputs.

You can increase the amount of options by using nested conditional operators. So this is like working around a bad syntax on a programming language.

```SystemVerilog
module mux4(
    input logic [3:0] d0, d1, d2, d3,
    input logic [1:0] s,
    output logic [3:0] y
); // You can see that now selecting a signal in a bus is like indexing an array. I think that it's very intuitive and can be applied to actual programming languages.
    assign y = s[1] ? (s[0] ? d3 : d2) :
    (s[0] ? d1 : d0);
endmodule
```

## 4.2.5 Internal Variables

Sometimes, you need to break a complex design into smaller chunks. To do this, we make internal variables. For example, the full-adder is a circuit with three inputs and two outputs. Here is the logical equation:

$$
S = A \oplus B \oplus C_{in}
$$
$$
C_{out} = AB + AC_{in} + BC_{in}
$$

We could actually define an intermediate variable and simplify the equation further by doing this:

$$
P = A \oplus B
$$

$$
G = AB
$$

And this would change the original full adder module into this:

$$
S = P \oplus C_{in}
$$

$$
C_{out} = G + C_{in}\left(A + B \right)
$$

$$
C_{out} = G + C_{in}P
$$

### Implementing this in SystemVerilog

```SystemVerilog
module fulladder(
    input logic a, b, cin,
    output logic s, cout
); // The things in the module declaration are basically the IO, but if it's not declared there, then it is an internal variable
    logic p, g;

    assign p = a ^ b;
    assign g = a & b;
    assign s = p ^ cin;
    assign cout = g | (cin & p);
endmodule
```

So the little weird thing that is a little specific to HDLs is that the order of declaration does not matter unlike a programming language where it does. This is because of the way the language is designed -> Not compiled for fast performance.

## 4.2.6 Precedence

Since there are multiple mainstream HDL, it is a good habit to parenthesize the order of operations, since the default operation order could be different depending on the language.

![](../../../assets/Pasted%20image%2020230703153327.png)

## 4.2.7 Numbers

Numbers can be specified in binary, octal, decimal, or hexadecimal. The size in bits of this number could also be specified, although it is an optional value. Just like in OCaml, underscores can be added to the number to break it so that it is more readable.

### SystemVerilog implementation

The format in SystemVerilog is `N'Bvalue` where `N` is the size in bits, `B` is the base and `value` is the value written in that base. In SystemVerilog, `'b` for binary, `'o` for octal, `'d` for decimal, and `'h` for hexadecimal.

When the base is not given, SystemVerilog will default to decimal. If the size is not given, then it will assume that it has as many bits as it needs to be used in the expression. Zeros are automatically padded on the front to match what it needs. For example, on a `6-bit` bus, `assign w='b11` gives `w` the value 000011. It's better practice to explicitly declare the size of the number. The one exception is that `'0` and `'1` are idioms used by SystemVerilog to fill all the bits with either 0s or 1s.

## 4.2.8 Z's and X's

HDLs use z to represent floating values.
- z are particularly useful for describing tristate buffers, who's output floats when the enable is 0.
- A bus could be driven by multiple tristate buffers, but only one can be active at a time.
- HDLs can represent this, and output a z when the output is a floating value.

HDLs will also use X to indicate invalid logic levels. When a bus is driven to both 0 and 1 at the same time by two enabled tristate buffers (or any other gates really), then the result will be x for contention.

At the start of simulations, state nodes like flip-flops are set to an unknown state (x). This is helpful for tracking bugs that are caused by flip-flops not being reset.

### Tristate buffer on SystemVerilog

```SystemVerilog
module tristate(input logic[3:0] a,
               input logic en,
               output tri [3:0] y);
    assign y en ? a : 4'bz;
endmodule
```

What does that `4'bz` mean? It is written as 4 bits binary float, but what it actually means is that all the lanes on the bus are floating. The tristate buffer is written as a `tri` type instead of `logic` because `logic` can only have one driver at a time. Tristate busses can have multiple drivers, so they should be declared as *net*. There are two types of nets in SystemVerilog, `tri` and `trireg`. So the difference is when there are no driver active on the bus, `tri` will float while `trireg` will assume the last value that the bus held. If no type has been declared, then `tri` is the default.

Another thing to note is that a `tri` output from a module can be taken as a `logic` input in a different module.

![](../../../assets/Pasted%20image%2020230703164128.png)

If a gate receives a floating input or an illegal input, it might output an x if it cannot determine what the correct output value should be.

## 4.2.9 Bit Swizzling

Sometimes, you will need to operate only on a subset of a bus or to concatenate signals to form busses. Both these operations are know together as *bit swizzling*.

### SystemVerilog implementation

```SystemVerilog
assign y = {c[2:1], {3{d[0]}}, c[0], 3'b101};
```

What did that mean? The `{}` is meant to concatenate busses. `{3{d[0]}}` indicate three copies of `d[0]`. `3'b101`  is a 3 bit binary constant. So in this example, `y` was given the 9-bit value $c_2c_1d_0d_0d_0c_0101$ using bit swizzling operations.

## 4.2.10 Delays

HDL statements may be associated with delays specified in an arbitrary unit. They are helpful in simulations to predict how fast a circuit will work, and also for debugging, and seeing what happens when you change the delay (deducing the source of a bad output that occurs in an unideal world).

Delay of a gate produced by the synthesizer depends on its $t_{pd}$ and $t_{cd}$ specification, which is not numbered in HDL code.

We are going to look at this Boolean equation again: $y=\bar{a}\bar{b}\bar{c} + a\bar{b}\bar{c} + a\bar{b}c$. That original function assumes that inverters have a delay of 1 ns, three-input AND gates have a delay of 2 ns, and three-input OR gates have a delay of 4 ns. The following figure shows that the output `y` has a lag of 7 ns.

![](../../../assets/Pasted%20image%2020230703183401.png)

At the beginning of the simulation, `y` will be initially unknown.

### SystemVerilog

How would we implement a delay on SystemVerilog? Well the sv files can include a timescale directive, as seen below:

```SystemVerilog

'timescale 1ns/1ps

module example(input logic a, b, c,
              output logic y);
    logic ab, bb, cb, n1, n2, n3;

    assign #1 {ab, bb, cb} = ~{a, b, c};
    assign #2 n1 = ab & bb & cb;
    assign #2 n2 = a & bb & cb;
    assign #2 n3 = a & bb & c;
    assign #4 y = n1 | n2 | n3;
endmodule

```

The timescale directive is the statement at the very top of the file, and the syntax is `timescale unit/precision`. By default, both `unit` and `precision` are 1 ns. The `#` are used to indicate the amount of units of delay. It can be placed in the `assign` statement, as well as nonblocking (<=) and blocking (=) assignments.

| [Previous](ch04_01_introduction.md) | [Home]() | [Next](ch04_03_structural_modeling.md) | 
| ----------------------------------- | -------- | -------------------------------------- |
