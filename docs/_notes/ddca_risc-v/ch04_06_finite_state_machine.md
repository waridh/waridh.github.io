---
title: "DDCA: 04.06 Finite State Machines"
topic: ddca
---

## 4.6.0 Finite State Machines

FSM requires a state register to track the state and two combinational logic block to move the machine into the next state and the output given the current state and input.

HDLs model the state machine by splitting it into three part:

1. **The state register**
2. **The next state logic**
3. **The output logic**

### Divide by three finite state machine implemented on SystemVerilog

```SystemVerilog
module divideby3FSM(
  input logic clk,
  input logic reset,
  output logic y
);
// Defining the enum types
typedef enum logic [1:0] {S0, S1, S2} statetype;
statetype state, nextstate;

// State register
always_ff @ (posedge clk, posedge reset)
  if (reset)  state <= S0;
  else        state <= nextstate;

// Next state logic
always_comb
  case(state)
    S0:       nextstate = S1;
    S1:       nextstate = S2;
    S2:       nextstate = S0;
    default:  nextstate = S0;
  endcase

// Output logic
assign y = (state == S0);

endmodule
```

So what is happening here?

- `typedef` assigned the `statetype` to be the `enum` logic type with two bits logic value and three variants: `S0`, `S1`, and `S2`. After defining this type, we then assign `state` and `nextstate` to this type.
- The `enum` ordering can be specified by the user as well. For example: `typedef enum logic [2:0] {S0 = 3'b001, S1 = 3'b010, S2 = 3'b100} statetype;`
- `case` statement is used to specify the state transition table, and it's done in the context of the next state logic needing to be combinational. The `default` case is necessary.
- For the output logic, it will output a `1` when the state is `S0` due to the equality. This part is very much like programming. Genuinely, you can write whatever conditional here, and it will output that value like it is a Boolean.

How this module works is that it will take the `reset` signal to initialize the state machine, and the machine will just move on the clock. It's really more like a slowing down of the clock. Counters would be more effective for that though.

In the `enum` cases, you can use conditional flow controls to determine the next case based on both the current case and the input that is given. Likewise the output could also be made to depend on both the state and the current input. Here are two examples of this:

### Pattern Recognizer Moore FSM

```SystemVerilog
module patternMoore(
  input logic clk,
  input logic reset,
  input logic a,
  output logic y
);
// Defining the three states
typedef enum logic [1:0] {S0, S1, S2} statetype;
statetype state, nextstate;

// State registers
always_ff @(posedge clk, posedge reset)
  if (reset) state <= S0;
  else state <= nextstate;

// Next state logic
always_comb
  case(state)
    S0: if (a)  nextstate = S0;
        else    nextstate = S1;
    S1: if (a)  nextstate = S2;
        else    nextstate = S1;
    S2: if (a)  nextstate = S0;
        else    nextstate = S1;
    default:    nextstate = S0;
  endcase

// Output logic
assign y = (state == S2);

endmodule
```

### Pattern Recognizer Mealy

```SystemVerilog
module patternMealy(input logic clk,
  input logic reset,
  input logic a,
  output logic y);

typedef enum logic {S0, S1} statetype;
statetype state, nextstate;

// State register
always_ff @(posedge clk, posedge reset)
  if (reset) state <= S0;
  else state <= nextstate;

// Next state logic
always_comb
  case(state)
    S0: if (a)  nextstate = S0;
        else    nextstate = S1;
    S1: if (a)  nextstate = S0;
        else    nextstate = S1;
    default:    nextstate = S0;
  endcase

// output logic
assign y = (a & state == S1);

endmodule
```

## 4.7 Data Types

This is a small section that could be tagged on here. Mostly about Verilog data types in case it has to be dealt with. Currently in SystemVerilog, we just use the `logic` type since it's very flexible and easy to deal with, but Verilog doesn't have this type. Instead, you have to use `reg` and `wire`. `reg` is used for variables that end up being used on the left hand side of a `<=` in an `always` statement. `wire` is the default type if you do not specify anything. More information for these two will be added if it was found that it is important.

Nets are the other major data type to be aware of. There is `wire` and `net`. In SystemVerilog, `wire` is very deprecated, while `tri` is used for gates with multiple drivers. `tri` will float(z) when not in use and output a conflicted(x) when the drivers are not set to the same. Here is a table of other nets for reference:

| Net Type | No Driver | Conflicting Drivers |
| --- | --- | --- |
| `tri` | `z` | `x` |
| `trireg` | previous value | `x` |
| `triand` | `z` | 0 if at least one 0 input (AND) |
| `trior` | `z` | 1 if at least one 1 input (OR) |
| `tri0` | 0 | `x` |
| `tri1` | 1 | `x` |
