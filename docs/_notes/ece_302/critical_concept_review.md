---
title: "Prerequisite Reviews"
topic: ece302
---

## Signals and systems

### System based approach

- When it comes to electronics, we use a system based approach to analysis and design.
  - A system is some sort of process that operates on an input and produce an output.

In this class, we are trying to learn how systems work conceptually, and "mechanically".

- Given the process and an input, what is the output? How do we find this output for an arbitrary input?

After learning about how systems works, we can then move on to designing.

- For a specified output and input, how do we design a system that will achieve this?
- Will this design work for all inputs? Usually not, since all electrical systems are not fully linear for all values of input. Some example includes adding two large values in a finite set, and having the result be outside the range of possible representation.

### Signals in electronic systems

- A signal
  - Is a time varying quantity (some function $$x\left(t\right)$$)
  - It conveys information
- Electronic signals may either be voltage ($$v\left(t\right)$$) or current ($$i\left(t\right)$$)
  - **Voltage** = The potential difference; i.e., the difference between the electrical potential between two points.
  - **Current** = The flow of electrical charge, or time rate of net charge flowing past a point of observation.
- **Electronics** = signal processing

## Frequency Spectrum

- Signals $$x\left(t\right)$$ convey information as some time-varying, measurable quantity.
- We use analysis, such as Fourier to take a signal and decompose it into a sum of sinusoidal signals of different frequencies; each component may be of different amplitude. In general, any signal $$x\left(t\right)$$ contains a spectrum of different frequency components.
- Electronic components and systems will have frequency-dependent response.

## Linearity

- We want our system to behave linearly.
- If a system can be defined as $$y=f\left(x\right)$$, a linear system will have the following properties:
  - **additivity** - if input a causes output f(a) and input b causes output f(b), then input $$a+b$$ will cause the output $$f\left(a + b \right) = f\left(a\right)+f\left(b\right)$$
  - **homogeneity** - $$f\left(ca\right) = cf\left(a\right)$$
  - Together, these are the principal of superposition.

The circuits that are analyzed and covered in ECE 302 can be considered linear within a reasonable bound, but like all systems, they are really all non-linear in the end.

- Non-linearity in electronics systems appear as distortions of the output spectrum. A single frequency input could cause output of not just the original input frequency, but other frequencies as well (usually harmonics of the input frequency).

## Signal Nomenclature

- Voltage
  - Relative measurements between two points.
  - By default, it is between any non-ground node and ground. Denoted with $$v_X\left(t\right)$$ where $$X$$ is the name of the node.
  - Relative voltage between two nodes is denoted with $$v_{XY}\left(t\right)$$ where $$X$$ and $$Y$$ are names of the node. The convention for reading this is $$v_{XY}\left(t\right) = v_{X}\left(t\right) - v_{Y}\left(t\right)$$.
    - This is also the voltage across an element if $$X$$ and $$Y$$ are the terminals of a two terminal circuit element.
  - Voltage has polarity: $$v_{XY}\left(t\right) = -v_{YX}\left(t\right)$$
- **Current**
  - The convention that is defined is that current is the flow of positive charge. (it is what it is)
  - There must be an assumed direction (sense) for the current flow when analyzing current.
    - If the value of the current found is positive, then the assumed direction was correct, if it is negative, then the assumed direction is incorrect and the current is moving in the opposite sense.
  - Current can either go into, or go out of a particular terminal of a device. In general, devices may have n terminals, where $$n \ge 2$$.
    - We can let current through a two terminal device.
