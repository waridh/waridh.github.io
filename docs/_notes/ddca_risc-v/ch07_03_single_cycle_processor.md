---
title: "DDCA: 07.03 Single Cycle Processor"
topic: ddca
excerpt: "Single cycle processors executes an instruction per cycle"
---

The chapter covers the microarchitecture by starting with the data-path, which is a combination of state elements and combinational logic that executes the instructions.

After this, we create the control unit, which outputs control signal that controls the data path based on the current instruction.

## 7.3.1 Sample Program

The processor that will be covered in this chapter will have loads, stores, an R-type instruction (or), and a branch (`beq`).

Some design choices:

- The program is stored in memory at the starting address `0x1000`. The following table shows the instructions that are present in this program.

| Address | | Instructions | Type | | | Fields | | | | |Machine Language |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `0x1000` | `L7:` | `lw x6, -4(x9)` | **I** | **imm{11:0}** `111111111100` | | **rs1** `01001` | **f3** `010` | **rd** `00110` | **op** `0000011` | `0xFFC4A303`
