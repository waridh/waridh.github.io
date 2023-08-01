---
title: "DDCA: 07.02 Performance Analysis"
topic: ddca
excerpt: "When testing processors, we should design a way to realistically measure the performance without just blindly looking at the clock speed and core count."
---

## Overview

Simply put, looking at just the raw spec of the CPU is not enough since some CPU are able to complete more in a cycle than others.

A more accurate way of looking that performance of a processor is to run the program that is going to be used on the machine, and log the performance of those.

- Sometimes, the program that is going to be ran on the machine has not been completed yet, so a similar program will have to suffice for performance analysis.
- A collection of programs made to determine the performance of a processor is called a *benchmark*, and they are used everywhere in tech.
  - Due to the rise of benchmarks, some hardware developers started to design their CPU in a way that would make their processor run uncharacteristically well. Thus, a performance analyst should take care to run many different benchmarks on the processor before publishing findings.
