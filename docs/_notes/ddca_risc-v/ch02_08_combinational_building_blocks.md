# 2.8 Combinational Building Blocks

Combinational building blocks come together to make more complex blocks.

## 2.8.1 Multiplexers

The most commonly used combinational circuits, they allow the choosing of the signal among multiple possible inputs. This is usually based around the value of the select signal.

### 2:1 Multiplexer

You have two inputs and a select signal. The number of select signals should be just enough to have enough combinations to cover all the input signal. Simple enough, here is the truth table:

| $D_0$ | $D_1$ | S   | Y   |
| ----- | ----- | --- | --- |
| 0     | 0     | 0   | 0   |
| 0     | 0     | 1   | 0   |
| 0     | 1     | 0   | 0   |
| 0     | 1     | 1   | 1   |
| 1     | 0     | 0   | 1   |
| 1     | 0     | 1   | 0   |
| 1     | 1     | 0   | 1   |
| 1     | 1     | 1   | 1   |

This makes sense.

### 4:1 Multiplexer built from 2:1 Multiplexers

We have already done a basic digital logic design course, so instead, we will have a look at doing modular builds.

