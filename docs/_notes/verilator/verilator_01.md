---
title: "Verilator: Basics Reference"
---

## Introduction To Verilator

Verilator reads Verilog and SystemVerilog code, along with some user created wrappers and outputs .cpp or .h files as "verilated" code. To simulate the design, just run the compiled executable. This program is recommended for those that need to simulate high speed designs.

Right now, I am mostly using this program for unit testing my SystemVerilog code.

## Usage Reference

### Installation

Verilator is most simple when used from UNIX, so if you own a Windows machine, install WSL/WSL2 and open up an Ubuntu terminal.

To install the software, simply input the following into the Ubuntu terminal:

```
sudo apt install verilator
```

You will also need some sort of C++ compiler. I'm currently using g++, but I do want to swap to clang when possible to use the LLVM.

### Usage

Verilator can be called using the `verilator` command, and it acts much like a compiler combined with a build system. There are nuance in the way the `verilator` command lines has to be used.

1. The files that are taken as input has to be done such that the dependent file must be input before, and thus the toplevel has to be input last.
2. You must make a C++ wrapper for the file that has the testcases and the testing infrastructure that verilator will build for you.

For example, you can start building with verilator by writing the following in the commandline terminal:

```
verilator -Wall -cc file1.sv file2.sv --prefix Someprefix --exe usercase.cxx -CFLAGS "-std=c++11"
```

- The `-Wall` flag is used to display all warnings
- The `-cc` flag specifies that we are working with C++ as opposed to SystemC.
- The SystemVerilog files that followed the `-cc` are the modules that are being used in order of the lowest level of dependencies to the top level module.
- The `--prefix` specifies the prefix that will be attached to the front of the verilated files. It's the same concept as modules in programming.
- The `--exe` specifies the user created C++ wrapper for the verilated code.
- The `-CFLAGS` is used to specify the version of C++ that should be used for the verilated code.

After the verilated code has been generated, you need to use the makefile created by Verilator to create the executable that will run the C++ wrapper that you wrote. This can be done by typing the following into the terminal:

```
make -j8 -f Someprefix.mk Someprefix
```

Where `-j8` is the amount of threads in the simulation, `-f` specify the makefile file, and `Someprefix.mk` is the makefile being read. `Someprefix` is the name of the executable that is being compiled.


## Note

Please note this is written as I am just starting out with Verilator. If there are some mistakes worth fixing, please don't hesitate to contact me through my email.
