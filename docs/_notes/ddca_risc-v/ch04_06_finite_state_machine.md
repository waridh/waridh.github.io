# 4.6 Finite State Machine

FSM requires a state register to track the state and two combinational logic block to move the machine into the next state and the output given the current state and input.

HDLs model the state machine by splitting it into three part.
1. The state register
2. The next state logic
3. The output logic

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

### Pattern Recognizer Moore FSM
