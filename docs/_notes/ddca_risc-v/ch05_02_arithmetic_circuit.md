---
title: "DDCA: 05.02 Arithmetic Circuits"
topic: ddca
---
## 5.2.1 Addition

Addition is made up of blocks that are dedicated to taking a few inputs and outputting a few inputs. There are a few of these small component blocks, so let's cover them quickly.

### Half Adder

- Takes two inputs `a`, and `b`, and then produce a sum `s` and a carry `cout`.
- The missing component of a half adder is that it doesn't take a carry in `cin`, and thus cannot be chained together to add N-bit values.

Here is a quick truth table:

| A | B | Cout | S |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

### Carry Propagate Adders

N-bit adder created from chaining multiple full adders together. There are three common implementation of a CPA, and those are ripple-carry adders, carry-lookahead adders, and prefix adders.

### Ripple-Carry Adders

- Simple implementation of a CPA.
- Easy to implement, as it is designed to be a linear chain of addition.
- Calculates the addition in linear time since the calculations done in the next significant bit is dependent on the calculations done in the previous full adder.

#### Time Calcuations

$$
t_{ripple} = Nt_{FA}
$$

where $$t_{FA}$$ is the time it takes to propagate through a full adder.

### Carry-Lookahead Adder

Has another concept built on top of the regular adder. Now the inputs are divided into blocks, and each block might be a carry generator, or a carry propagator. Here are the quick definitions for them.

#### Generator

The block is always going to send out a carry, regardless of if there is going to be a carry input. This might happen if the column at the MSB of the block has `a` and `b` both be `1` since it will create a carry out regardless of the carry in.

#### Propagator

This block is going to output a carry out if it has a carry in

The higher level thinking here is that, with the concepts of generators and propagators, you can determine if a bit is going to generate a carry by using the following Boolean equation:

$$
C_i = A_iB_i + \left( A_i + B_i\right)C_{i-1} = G_i + P_iC_{i-1}
$$

Quick note: $$G_i$$ and $$P_i$$ are the notation for generators and propagators. Now to push this into the block segmentation, the following Boolean algebra models the generation of a block:

$$
G_{3:0} = G_3 + P_3\left( G_2 + P_2\left( G_1 + P_1G_0\right)\right)
$$

A block will propagate if all the bits inside the block also propagate:

$$
P_{3:0} = P_3P_2P_1P_0
$$

Now just like how you can determine if a column is going to have a carry out, we can do the same thing with a block using the following Boolean expression:

$$
C_i = G_{i:j} + P_{i:j}C_{j-1}
$$

So the benefit is that we can determine the carry out of this block and thus the carry in of the next block without having to go through the full adder circuit, thus, skipping much of the chain (Effectively only needing to go through two two-input gates). Even with the increased speed, it is still linear complexity as a more significant block will still need the carry out from a less significant block.

Now the block still have to undergo some ripple-carry adders, inside, but there is less chaining in there now.

#### CLA Time Propagation

$$
t_{CLA}=t_{pg} + t_{pg\_block}+\left( \frac{N}{k}-1\right)t_{AND \_ OR}+kt_{FA}
$$

##### Definitions
- $$t_{pg}$$
  - This is the time for a column propagate and generate signal to be made (Done for all the columns at once).
- $$t_{pg\_block}$$
  - The time it takes to find a block generate or propagate signal.
- $$t_{AND\_OR}$$
  - The final delay from carry in to carry out in a block level. This is just an AND gate and an OR gate.
- $$t_{FA}$$
  - This is the full adder delay.
