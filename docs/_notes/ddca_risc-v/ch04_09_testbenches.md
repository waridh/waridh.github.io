---
title: "DDCA: 04.09 Testbenches"
topic: ddca
excerpt: Some brief view on what test benches are, and how you can design them. Similar to unit tests in higher level programming.
---

## 4.9 Testbenches

This is arguably the most important skill to come out of this chapter. As long as you can test your result, you can solve the rest. Especially since this is pre-silicon stuff, we should be abusing test benches.

### How it works

In HDLs, testing is done using a module. This test module will generally follow these steps:

1. Instantiate the variables of the inputs, output and expected output.
2. Use the `initial` statement to start the process of assigning test input cases.
3. Check the output of the DUT to see if it matches the expected output.

Make sure to design appropriate delays in such that there is no confusion.

There are simple ways to create test modules, and there are robust ways of making test modules. The simple method requires the user create the test cases in the module itself, and use `assert` statement to check if the output is correct. The robust method requires the use of an external text file that will be read by SystemVerilog. This file contain binary values which corresponds to inputs and expected output. A clock is created in this more complex test case, and we essentially simulate a while loop that just checks a function with different inputs and expected outputs.

The following are example code for both:

### Self-checking Testbench

```
module testbench2();
// Instantiate the variables
  logic a, b, c, y;

  // Instantiate the DUT
  sillyfunction dut(a, b, c, y);

  // Testing sequentially followed by result check
  initial begin
    a = 0; b = 0; c = 0; #10;
    assert (y === 1) else $error("000 failed.");
    c = 1; #10;
    assert (y === 0) else $error("001 failed.");
    b = 1; c = 0; #10;
    assert (y === 0) else $error("010 failed.");
    c = 1; #10;
    assert (y === 0) else $error("011 failed.");
    a = 1; b = 0; c = 0; #10;
    assert (y === 1) else $error("100 failed.");
    c = 1; #10;
    assert (y === 1) else $error("101 failed.");
    b = 1; c = 0; #10;
    assert (y === 0) else $error("110 failed.");
    c = 1; #10;
    assert (y === 0) else $error("111 failed.");
  end
endmodule
```

This method took a lot of writing, so it might be more scalable to use the following method, it's just that on simpler cases, it may take more time to set up. There are going to be two components, and you can see them now:

### Testbench with Test Vector File

#### Test Vector File

```
000_1
001_0
010_0
011_0
100_1
101_1
110_0
111_0
```

#### Testing Module

```
module testbench3();
  logic clk, reset;
  logic a, b, c, y, yexpected;
  logic [31:0] vectornum, errors;
  logic [3:0] testvectors[10000:0];

  // Instantiating dut
  sillyfunction dut(a, b, c, y);

  // Generate clock
  always
    begin
      clk = 1; #5; clk = 0; #5;
    end
  
  // Loading the vector at the start of test along with resetting
  initial
    begin
      $readmemb("example.txt", testvectors);
      vectornum = 0; errors = 0;
      reset = 1; #22; reset = 0;
    end

  // apply test vectors on rising edge of clk
  always @(posedge clk)
    begin
      #1; {a, b, c, yexpected} = testvectors[vectornum];
    end
  
  // checking the result on the falling edge
  always @(negedge clk)
    if (~reset) begin
      if (y !== yexpected) begin
        $display("Error: input = %b", {a, b, c});
        $display(" outputs = $b (%b expected)", y, yexpected);
        errors = errors + 1;
      end
      vectornum = vectornum + 1;
      if (testvectors[vectornum] === 4'bx) begin
        $display("%d tests completed with %d errors.", vectornum, errors);
        $stop;
      end
    end
```

After some testing, it is found that you should be abusing for loops in order to quickly automate your input. By doing this, you can also quickly write out your debugging information.

### Note with Vivado

There is a new bigger can of worm to deal with when using vivado to do your testing. Essentially, the notes taken above does not work exactly like entailed, since asserts and error counts will not work using simulation. Otherwise, it's pretty similar to the first example, only that we use more for loops. (Just be cleaver programmer, you should be fine.) The TODO that we just got now is understanding Vivado constraints. The one benefit of this is that it uses Tcl as the language, and I happen to know Tcl.
