---
title: "DDCA: 07.01 Introduction to Microarchitectures"
topic: ddca
excerpt: "Microarchitecture is how each modules and components of a processor come together and interact with each other."
---

How do we put together a processor? What features do we have to try and balance together to get the most out of the money we spend manufacturing the microprocessor? How do we put together the ALU, FSM, registers and memory? This chapter covers a little of all of these topics.

## 7.1.1 Architectural State and Instruction Set

A computer architecture is defined by the instruction set and the architecture state.

- The architecture state is essentially the state in a finite state machine.
  - In RISC-V, this is the program counter and the 32 32-bit registers. Part of the RISC-V specification requires that all RISC-V processors have these components.
- Using the current architecture state, the processor can determine what instruction to execute, and the next architecture state that it should assume.
- There are some non-architectural states that are inside the architecture state machine, used for optimization or simplification of logic.

### Instruction Subset of focus

In the scope of this textbook, four types of instructions are going to be looked at. These are:

- **R-type** - `add, sub, and, or, slt` : These are three register operations.
- **Memory instructions** - `lw, sw` : These are basic memory instructions.
- **Branches** - `beq` : Branching can be used as a way to implement conditionals, or functions.

The point here is that, you should be able to make some programs using just these three types of instructions.

## 7.1.2 Design Process

A microarchitecture can be divided into two parts:

- The *control unit*
  - Takes the current instructions from the data path, and then tells the data path what to do.
  - Control of the data path is done by generating *mux select*, *register enable*, and *memory write* signals.
- The *data path*
  - Operates on words of data.
  - Has memories, registers, ALUs, and multiplexers.
  - RISC-V uses 32-bit data path.

Since CPUs are complex, we want to start the design process from the top down, which means starting with the state elements, since they control the flow of the architecture state and memory. After the state element has been created, blocks of combinational logic is added between these state elements so that the next state can be reached from the current state.
