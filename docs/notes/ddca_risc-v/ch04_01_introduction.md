## 4.1.1 Modules

A block of hardware with inputs and outputs is called a *module*. AND gates, multiplexers, and a priority circuit are all hardware modules. There are two methods of describing module functionality:
1. *behavioural* - What the module does
2. *structural* - How it is built from simpler pieces. An application hierarchy.

Modules in SystemVerilog acts a lot like functions. You define your inputs and outputs, and then you write the logic inside of it.

## 4.1.2 Language Origins

You should be fluent in both languages. Some universities will teach VHDL while others will teach SystemVerilog. Personally, my university will teach my VHDL, so I am going to focus my attention on SystemVerilog for now.

### SystemVerilog History

Verilog was developed by Gateway Design Automation as a proprietary language for logic simulation in 1984. It did not stay that way when Cadence purchased Gateway Design Automation in 1989. in 1990, Verilog was made an open standard. It became an IEEE standard in 1995, and then the language was further extended in 2005. All these extensions was them merged into a single language, now called SystemVerilog. The extension for SystemVerilog is `.sv`.

Compared to SystemVerilog, VHDL is more verbose and cumbersome.

## 4.1.3 Simulation and Synthesis

The two main purpose of HDLs are logic simulation and synthesis. During a simulation, inputs are applied to the module, and the output are checked to verify that the module are operating correctly. During synthesis, the textual description of a module is transformed into logic gates.

### Simulation

Humans routinely makes mistakes, and such errors in hardware design are called bugs. Getting rid of bugs in a digital system is important because it prevents paying customers from complaining. Testing a pre-silicon system in a lab is time consuming. Discovering the cause of the error in a lab is very difficult as well because only signals routed to the chip pins can be observed.

Since you cannot see what is happening inside a chip when in a physical lab, it is useful to create RTL simulations. From my time in industry, I know that it is very expensive to make a fix after the silicon product had already started.

### Synthesis

Logic synthesis transforms HDL code into a *netlist* describing the hardware (e.g., the logic gates and the wires connecting them). There might be optimization being done by the HDL to reduce the amount of hardware being used. This is like using a K-Map to reduce logic.

HDL circuits description are like code in a programming language, but this code is intended to represent hardware. There are many commands built into SystemVerilog and VHDL.

Not all the commands that are available in HDLs can be synthesized into hardware. For example, the command to print result into the screen during simulation is not going to be translated into hardware. Since we are trying to build hardware, we are going to really learn the synthesizable subset of the language. HDLs can be divided into *synthesizable* and *testbench*. The synthesizable modules describe the hardware, while the testbench code are used apply inputs to a module and check if the output are correct. This is the verification bits. Writing these up yourself is an important skill.

An important note is that we should be seeing HDLs as a shorthand for describing hardware and not a computer program. You have to make sure that you create something that is makable in real life. We need to think of the system in terms of combinational logic, registers, and finite state machines instead of functions and objects. Actually, the thing is there are some similarities.

We are going to be going through a lot of examples for HDLs, so hopefully, we will understand how to make it work.

*Idioms* are the specific way in which HDLs describe various classes of logic.

| Previous | [Home]() | [Next](ch04_02_combinational_logic.md) | 
| -------- | -------- | -------------------------------------- |
