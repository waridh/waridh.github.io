---
title: "Arithmetic Circuits"
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
