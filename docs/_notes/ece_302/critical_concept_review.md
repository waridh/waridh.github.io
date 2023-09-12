---
title: "Transistor Physics Prerequisite Reviews"
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
- Power Dissipation
  - Power dissipation of a two terminal device can be defined as $$p\left(t\right)=v\left(t\right)i\left(t\right)$$ using the convention for voltage and current as defined above.
  - If the power is negative, it means that the device is absorbing power from the system.
  - If the power is positive then the element is supplying power to the system.
  - Power dissipation of an n-terminal device is given by the following generalized equation: $$p\left(t\right)=\Sigma v_{node}\left(t\right)i_{in}\left(t\right)$$
  - Even in linear circuits, power should be treated as a non-linear function of voltage and/or current.
- **Total instantaneous, DC, and AC signals**
  - There is a convention for communicating if the value being referenced is T.I., DC, or AC.
    - T.I. - The measurable signal. $$v_C\left(t\right)$$
    - DC - Constant with respect to time, thus not a function of time. $$V_C$$.
    - AC - Time varying component of the signal. Denoted with the following: $$v_c\left(t\right)$$
    - Finally $$T.I. = DC + AC$$
    - $$v_C\left(t\right) = V_C + v_c\left(t\right)$$

## Current-voltage characteristics

- Inductors
  - $$ v_L\left(t\right) = L\frac{d}{dt}i_L\left(t\right)$$
  - $$ i_L\left(t\right) = \frac{1}{L}\int_{0^+}^tv_L\left(t\right)dt+i_L\left(0\right)$$
  - The current cannot jump
  - Store energy in magnetic fields
- Resistor
  - $$v_R\left(t\right)=Ri_R\left(t\right)$$
- Capacitor
  - $$ i_C\left(t\right) = C\frac{d}{dt}v_C\left(t\right)$$
  - $$ v_C\left(t\right) = \frac{1}{C}\int_{0^+}^ti_C\left(t\right)dt$$
  - The voltage cannot jump
  - Store energy in electric fields
- The main model we use for circuit analysis uses the lumped component model with ideal components.
  - What this entails is that the inductors do not have parasitic capacitance, and the wires and terminals have no resistance.

## Components

- Short circuit
  - Simply an ideal wire. There is no voltage difference when measuring the two terminals of this element.
  - $$v\left(t\right)=0 \forall i\left(t\right)$$
- Open circuit
  - A lack of wire between two points. There may be a voltage difference, but there will be no current going through.
  - $$i\left(t\right)=0 \forall v\left(t\right)$$
- Resistor (ideal)
  - A basic resistor. Follows Ohm's law. A linear component.
  - $$v\left(t\right)= i\left(t\right)R$$
  - Short circuit is equivalent to $$R=0$$
  - Open circuit is equivalent to $$R=\infty$$
- Constant voltage source
  - Element that maintains a constant voltage across its terminals.
  - $$v\left(t\right)=V_{DC} \forall i\left(t\right)$$
  - When this element is scaled to zero, we have a closed circuit.
- Voltage signal generator
  - Element that generates a time varying signal across its terminals.
  - $$v\left(t\right)=v_{ac}\left(t\right) \forall i\left(t\right)$$
  - When this element is scaled to zero, we have a closed circuit.
- Constant current source
  - Element that maintains a constant current flowing through it.
  - $$i\left(t\right)= I_{DC} \forall v\left(t\right)$$
  - When this source is scaled to zero, then we have an open circuit.
- Current signal generator
  - Element that generates a time varying current signal.
  - $$i\left(t\right)= i_{ac}\left(t\right) \forall v\left(t\right)$$
  - When this source is scaled to zero, then we have an open circuit.

## Circuit topology

### Definitions

- **Parallel**
  - The elements have the same set of two terminals.
  - The total current is the sum of all the current going through all the elements in parallel.
  - You measure the voltage of a system by putting the voltmeter in parallel with the element. In an ideal case, the voltmeter is an open circuit.
  - The resistance of a system is parallel can be represented with the following expression.
    - $$\frac{1}{R_{parallel}} = \sum_{i=0}^{N}\frac{1}{R_i}$$
    - $$R_{paralllel} = R_1 \parallel R_2$$
    - That second notation handles the math conversion that is needed to get from the inverted form to the regular form of the parallel resistance.
- **Series**
  - The current is the same going through all the elements in series.
  - The total voltage of a series system is the sum of the voltage of each element.
  - You measure the current of a system by putting an ammeter in series with a wire. In the ideal scenario, the ammeter is a short circuit with no resistance.
  - The resistance of a system is series is just the straight sum.
    - $$R_{series} = \sum_{i=1}^{N}R_i$$

### Reduction methods

- **Voltage division**
  - Works on elements that are in series.
  - $$\frac{v_{R_1}\left(t\right)}{R_1}=\frac{v\left(t\right)}{R_1+R_2}$$
  - $$v_{R_i}\left(t\right)=\frac{v\left(t\right)\times R_i}{\sum_{j=1}^{N}R_j}$$
- **Current division**
  - More complicated to do, but there might be places where you should use it.
  - Works on elements that are in parallel.
  - $$i_{R_1}\left(t\right)=i\left(t\right)\frac{R_1\parallel R_2}{R_1}$$
  - $$i\left(t\right)\frac{R_1\parallel R_2}{R_1}= i\left(t\right)\frac{R_2}{R_1+R_2}$$
  - Need to reduce recursively when working with more than just a pair of parallel resistors.

## Networks

### Port

A port is a pair of terminals that connects a network to the outside. They are the black box interface of the network. There needs to be two because circuits need to make a loop in order to be active.

- The convention is that current enters through the +ve terminal and exits through the -ve terminal.
- The current that enters a port is the same as the current that exits the port.

### One port networks

Basically an isolated element. Can be used to reduce a more complicated system down to a more simple form.

### Thevenin's

- On a one port linear circuit, we can reduce the inside to a circuit with just one voltage source and one resistor.
  - The convention is $$ V_{TH} $$ and $$ R_{TH} $$
- The $$V_{TH}$$ is given by the open circuit voltage of the port.
- The $$R_{TH}$$ is found by turning off all independent sources in the network and measuring the resistance.

### Norton's

- The dual to Thevenin. Uses current instead of voltage.
- Simplifies a one port linear network down to a current source in parallel with a single resistor.
- The value of the resistor is found the same way as $$R_{TH}$$.
- The Norton current is found by short-circuiting the ports.
  - The current source is placed in parallel with the resistor.

### TH N duality

$$
R_N = R_{TH} = \frac{v_{TH}\left(t\right)}{i_N\left(t\right)} = \frac{v_{OC}\left(t\right)}{i_{SC}\left(t\right)}
$$

### Special cases

#### No independent sources

- The Thevenin/Norton circuit will reduce to just a resistor.

#### One or more dependent sources

- Need to apply an independent source to the port to "turn on" the circuit.
  - Find $$R_{TH}$$ by taking the ratio of voltage to current for the independent source.
  - This only works because we are assuming that we are working with a linear circuit. This means that the model will start breaking down when the network is not linear.

### Other notes

Both signal generators have a resistor that is associated with it. In the ideal scenario, these resistances are just not interacting with the network, but the reality is that this is not the case.

### Two port networks

Many electronics black boxes are actually two port networks. One port acts as input, and the other as output.

- The same rules applies. Each port with have their own port voltage, and the current entering a port is the same as the current exiting the same port.
  - The other port does not have to have the same values, but it must be consistent with itself.
- For two port networks that do not contain any independent sources, there is a set of four parameters that can describe the relationship between $$v_1\left(t\right)$$, $$v_2\left(t\right)$$, $$i_1\left(t\right)$$, and $$i_2\left(t\right)$$
  - The parameters are given as a $$2\times2$$ matrix.
  - You choose two of the variable as an input vector, when when matrix multiplied with the matrix, yields an output.

Two common ones are impedance and admittance.

$$
\begin{bmatrix}V_1\left(s\right) \\
V_2\left(s\right)\end{bmatrix} = \begin{bmatrix}z_{11} & z_{12} \\
 z_{21} & z_{22}\end{bmatrix}\begin{bmatrix}I_1\left(s\right) \\
I_2\left(s\right)\end{bmatrix}
$$

$$
\begin{bmatrix}I_1\left(s\right) \\
I_2\left(s\right)\end{bmatrix} = \begin{bmatrix}y_{11} & y_{12} \\
 y_{21} & z_{22}\end{bmatrix}\begin{bmatrix}V_1\left(s\right) \\
V_2\left(s\right)\end{bmatrix}
$$

Where the impedance and the addmittance matrix are inverses.

$$
\left[z\right]^{-1} = \left[y\right]
$$

### Two port network chains

There are three types of chaining that is possible with two port networks.

- Cascade
- Series
- Parallel (Shunt)

#### Cascade

This is the simple type of chaining that you would expect. You simply connect the output port of one network with the input port of another network.

#### Parallel

The two network are connected in a way where both their port voltage is the same.

#### Series

The two networks are connected in way where both their ports have the same current flowing through.

#### Other commbination

It is not necessary that both port of a two port network connected with another two-port network has to be connected in the same configuration. For example, one set of port could be connected in series and the other one connected in parallel.

## Circuit analysis methods

### Loop analysis

Split the system into loop, and use series analysis to solve for each component.

#### How to use it

- You are solving for a the current for each loop.
- The sum of voltage for each loop will be equal to zero.

Here are some of the convetion that I am using for this method of analysis.

- After drawing a path, if you hit a voltage source, put them on the LHS of the equation. If the current you are using is entering from the positive terminal, set the value to be negative as this represents a voltage drop. If the current is entering from the negative terminal, then set the value to be positive since it is a voltage gain (pushing the current forward).
- If the current hits a resistor, put the product of the current and the resistor on the RHS of the eqn.
- When there is another loop opposite the element that is producted, make sure to sum the current of that loop and this loop together before taking the product with the element. This would be the corrent representation of the current that is going through the element.
- If we have a current source that is only feeding into one loop, then we know the current that is going through the loop.

### Nodal analysis

- A minimal method of find the solution to certain problems. The equations of this method might be more messy than loop analysis, but you can genuinely do a lot less work.

#### Parts

- **Nodes**
  - Every network has nodes.
  - In this method of analysis, the goal is to final all the independent nodes in the system.
  - If two adjacent nodes are connected by a voltage source, then they can be combined to form a super-node.
- **Ground**
  - The ground is a reference node that has the voltage of 0V.

#### Process

- For every independent nodes, invoke KCL in that the amount of current entering the node is the same as the amount that is leaving the node.
  - $$\sum i_{in}\left(t\right)=\sum i_{out}\left(t\right)$$
- On current sources, you will already have the direction and the value, so you simply have to place it on this equation for the node.
- To find the current between two nodes where there isn't a current source, you can assume that the current is always leaving or entering the node being analyzed, and just take the voltage difference between the node and divide it by the resistance between them.
  - $$\frac{v_{\text{this node}}\left(t\right)-v_\text{adjecent node}\left(t\right)}{R_\text{branch}}$$
