---
title: "DDCA: 01.08 Power Consumption"
tags: ddca computer_architecture notes

---

Power consumption is important for digital system, and has immediate applicability in the products that are created using these systems. Smart phones, and laptops need to be able to operate for many hours of the day.

The way that power is conveyed is in Watt-hour. A 10 Watt hour battery could deliver 1 watt in 10 hours, or 2 watt in 5 hours. So when we say that our laptop is a 100 W-hr battery, it would have to consume less than 10 W in an hour to be able to last 10 hours. Even when plugged in, power is an important consideration as you still need to pay for it.

## Integration with Digital Systems

Digital systems draw both *static* and *dynamic* power.
- **Dynamic Power**: The power used to charge capacitors when it changes from 0 to 1.
- **Static Power**: The power that is always used, even when the gates are closed, or when the system is IDLE.

Logic gates and the wires that connect them have capacitance. The energy required to charge a capacitance $$C$$ to $$V_{DD}$$ is $${CV_{DD}}^2$$. If a system operates at a frequency $$f$$, and the fraction of the cycles on which the capacitor charges and discharges is $$\alpha$$, (also called the **activity factor**), then the dynamic power consumptions is defined by the following equation.

$$
P_{\text{dynamic}=\alpha C{V_{DD}}^2f}
$$

### Clock diagram

![](/assets/Pasted%20image%2020230618183400.png)

In a), the signal is rising and falling once every clock signal. The activity factor for this signal is then 1. The clock period between rising edges is called $$T_c$$, and it is the reciprocal of the frequency $$f$$. In b), the signal is changing once every clock cycle, and thus, since there is only one activity per cycle, the $$\alpha$$ is set to 0.5, as half a rising, and half a falling. in c), the activity has moved on to once every other clock, and this will cause the $$\alpha$$ to be 0.25. You can think of $$\alpha=1$$ as when there are two changes in a single clock, and do that fractional math from there with rates and stuff.

Real digital systems will have components that are not switching, so an activity factor of 0.1 is more typical.

## IDLE Power Draw

Even when the components are idle, electrical system will still have some power draw that occurs. Some circuits will have a path from $$V_{DD}$$ to ground, which will cause power to be drawn continuously, such as a pseudo-nMOS gate.

The total static current, $$I_{DD}$$ is also called the **leakage current** or the **quiescent supply current** flowing between the $$V_{DD}$$ and $$GND$$. The static power consumption is proportional to $$I_{DD}$$.

$$
P_{\text{static}}=I_{DD}V_{DD}
$$

## Practice Problems

### Power Consumption

A particular cell phone has an 8W-hr battery and operates at 0.707V. Suppose that, when it is in use, the cell phone operates at 2GHz. The total capacitance of the circuitry is 10 nF (10−8 Farads), and the activity factor is 0.05. When voice or data are active (10% of its time in use), it also broadcasts 3W of power out of its antenna. When the phone is not in use, the dynamic power drops to almost zero because the signal processing is turned off. But the phone also draws 100mA of quiescent current whether it is in use or not. Determine the battery life of the phone (a) if it is not being used and (b) if it is being used continuously.

#### Answer

![](/assets/powerex-5.jpg)



