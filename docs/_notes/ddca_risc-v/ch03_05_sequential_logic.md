# 3.5.4 Metastability

In asynchronous inputs, you cannot guarantee that the input will come in at a time that isn't overlapping with the aperture. What happens when the input changes between $t_{setup}$ and $t_{hold}$? This input would violate the dynamic discipline, and the output would be undefined.

## Metastable State

When a flip-flop takes an input reading and the input is changing during the aperture, then the output will be between $0$ and $V_{DD}$ and potentially be in the forbidden zone. When it is in this zone, then it is what is called *metastable*. The reason it is called metastable is that the output could be balanced on this state until something comes to push it to the stable state, and the duration in this state is unbounded, meaning it could be indefinite.

![](../../../assets/Pasted%20image%2020230705140909.png)

## Resolution Time: $t_{res}$

This is the time it takes the output to change from a metastable state to a stable state. If the input changes outside the aperture, then the $t_{res}$ will be the same as $t_{pcq}$. If the input happens in the aperture time, then $t_{res}$ could be much longer. Here is the probability density function for the random variable:

$$
P\left( t_{res} > t \right) = \frac{T_0}{T_c}e^{- \frac{t}{\tau}}
$$

Where $T_c$ is the clock period, $T_0$ and $\tau$ are characteristic of the flip-flop. The equation is valid only for when $t$ is substantially longer than $t_{pcq}$.

The $\frac{T_0}{T_c}$ represents the probability that a conflicting asynchronous input will happen, and the probability decreased with the increase in $T_c$. $\tau$ is the time constant indicating how fast the flip-flop can move away from the metastable state. It has the do with the delay through the cross-coupled gates in the flip-flop.

# 3.5.5 Synchronizers

In the real world, not all inputs are going to be synchronous, for example, human inputs are guaranteed to be asynchronous. If the system is not build to handle those types of input, failures and lock ups can happen. Although not completely preventable, there are ways of minimizing the chances of creating erratic behaviour in a systems like this. One of those ways are *synchronizers*.

Functionally, synchronizers will take an asynchronous input D, and a clock CLK. It makes an output Q within a bounded amount of time<!--Specified by the clock??-->. The output will have a valid logic level with an extremely high probability. If D is stable during the aperture, Q should take on the same value as D. If D changes during the aperture, Q may either take on a HIGH or LOW value, but must not be metastable. Basically, the device is made just to avoid getting into a metastable state.

A simple implementation is by using two flip-flops, and line them up as such:

![](../../../assets/Pasted%20image%2020230705161831.png)

I idea is that if you want long enough on the second flip-flop, it is likely that any metastable state that was created by the first flip-flop will fall back into a stable state. When the period is over, the second flip-flop will sample the input from the first flip-flop and it will be likely that the intermediate variable has fallen into a stable state by now. The second flip-flop will now product a good output $Q$.

$$
P\left(failure\right) = \frac{T_0}{T_c}e^{-\frac{T_c-t_{setup}}{\tau}}
$$

The probability of failure is the probability that the final output $Q$ will be metastable upon a single change in D. If D changes once per second, then the probability of failure per second is going to be just P(failure), but when D changes N times per second, the new P(failure) per second gets multiplied by the amount of changes:

$$
P\left(failure\right) = N\frac{T_0}{T_c}e^{-\frac{T_c-t_{setup}}{\tau}}
$$

When doing system reliability, we look at the indicator of *mean time between failure* ($MTBF$). This is just the average time between failure and it's just the reciprocal of the probability that a system will fail in a second:

$$
MTBF = \frac{1}{P\left(failure\right/sec)} = \frac{T_ce^{\frac{T_c-t_{setup}}{\tau}}}{NT_0}
$$

The $MTBF$ improves at an exponential rate as $T_c$ increase. So the longer the period before the second flip-flop samples the output from the first, the less likely that the output is going to be in a metastable state.

In a lot of systems, they will just wait one clock cycle before sampling the intermediate value.