---
title: "DDCA: 04.09 Testbenches"
topic: ddca
---

## 4.9 Testbenches

This is arguably the most important skill to come out of this chapter. As long as you can test your result, you can solve the rest. Especially since this is pre-silicon stuff, we should be abusing test benches.

### How it works

In HDLs, testing is done using a module. This test module will generally follow these steps:

1. Instantiate the variables of the inputs, output and expected output.
2. Use the `initial` statement to start the process of assigning test input cases.
3. Check the output of the DUT to see if it matches the expected output.

Make sure to design appropriate delays in such that there is no confusion.
