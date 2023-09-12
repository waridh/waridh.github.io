---
title:"ECE 340: Descrete Time Signals"
topic:ece340
---

## Discrete Time Signal, Analog Signals, and Digital signals

- Discrete Time (DT) signals are non-continuous time signals, in that they are sampled in a discrete way.
- Digital signals are DT signals that also have discrete amplitude.

## Periodicity

The periodicity of a discrete signal is much like those of continuous time signals, with one main difference. The calculated period of the signal has to be a rational value.

The following is the equation that can be used to find the period of a discrete sine wave.

$$
x\left[k\right] = \sin\left(\Omega_0k+\theta\right)
$$

$$
\text{The period } K = \frac{2\pi}{\Omega_0}.m
$$

Where $$K$$ is the period and the number of cycles is the denominator of the reduced fraction. If the value is not a rational number, then the signal is not periodic.

## Energy signal

- The instantaneous energy can be obtained with $$\left|x\left[k\right]\right|^2$$.
- The total signal energy can be obtained with $$E_x=\sum_{k=-\infty}^{\infty}\left|x\left[k\right]\right|^2$$
  - The signal is not an energy signal if the total energy of the signal is $$\infty$$, which usually means that the signal is either periodic or on a cycle.

## Power signal

- A way to a signal by their average energy. Useful on periodic signals.
- On a periodic signal, you need to have the period.
- $$ P_x = \frac{1}{K}\sum_{k=0}^{K-1}\left|x\left[k\right]\right|^2 $$
- When $$0 < P_x < \infty$$, the signal is a power signal.

## Deterministic vs Random signals

- Deterministic signals are signals that are signals that can be predicted using an equation.
- Random signals are signals that cannot be predicted. Most signals are random, but we are going to be looking deterministic signals in this course.

## Even, odd, and neither-even-nor-odd signals

- Even signals are signals that are fully symmetric around $$k=0$$.
- Odd signals are signals that are odd symmetric around zero.
- If neither of these definitions apply, then the signal is neither even nor odd.

An example of an even signal:

$$
x\left[k\right] = e^k+e^{-k}
$$
$$
x\left[-k\right] = e^{-k}+e^{-\left(-k\right) } = e^k+e^{-k} = x\left[k\right]
$$

An example of an odd signal:

$$
x\left[k\right] = \sin\left(\frac{\pi}{8}k\right)
$$

$$
x\left[-k\right] = \sin\left(\frac{\pi}{8}\left(-k\right)\right) = -\sin\left(\frac{\pi}{8}k\right) = -x\left[k\right]
$$

There are four properties of even and odd signals that can be abused. These are them.

1. $$\text{odd seq} \times \text{odd seq} \longrightarrow \text{even seq}$$
2. $$\text{even seq} \times \text{odd seq} \longrightarrow \text{odd seq}$$
3. $$\sum_{k=-M}^{M}g_o\left[k\right] = 0$$
4. $$\sum_{k=-M}^{M}g_e\left[k\right] = g\left[0\right]+2\sum_{k=1}^{M}g_e\left[k\right]$$

## Some elementary DT signals

1. Unit impulse function. The base function is just a value of 1 at $$k=0$$. $$\delta\left[k\right]=\begin{cases}1 & k=0 \\
0 & k \ne 0
\end{cases}$$
2. Unit step function. Works similary to continuous time version, but it's now many DT 1s instead of a continuous block. It can be described with the following piecewise function. $$u\left[k\right]=\begin{cases}1 & k \ge 0 \\
0 & k < 0\end{cases}$$
3. Rectangular function. $$rect\left(\frac{k}{2N+1}\right)=\begin{cases}1 & \left|k\right|\le N \\
0 &\left|k\right|\gt N \end{cases}$$
4. Sinc. In this scope of this class, we are going to be using the following definition. $$ sinc\left(\Omega_0k\right) = \frac{\sin\left(\pi\Omega_o k\right)}{\pi\Omega_o k} $$

## Complex Exponential function

Using Euler's numbers, we have the following equivalence.

$$
e^{j\Omega k} = \cos\left(\Omega k\right) + j \sin\left(\Omega k\right)
$$

There are two ways to graph complex exponential functions, and that is either by using two graphs, one real, one imaginary, or use two graphs where one is the magnitude and the other is the phase.

## Signal Operations

Going to cover the following three concepts:

1. Time shifting
2. Time scaling
3. Time inversion

### Time shifting

This simply moves the entire signal either forwards or backwards in time. Since I get confused easily by forwards and backwards, I am going to describe it in shifting left and right for now.

$$
x\left[k-4\right] \text{shifts the signal right}
$$

### Time scaling

Since the signals are discrete now, there are differences in the scaling from continuous is that when scaling up, there are missing data, and you must interpolate to fill in the missing time. When you are scaling down, you are actually dropping samples in order to achieve the new signal.

Here is an example of scaling down:

$$
y\left[k\right] = x\left[2k\right]
$$

example of scaling up (might need interpolation after doing so):

$$
y\left[k\right] = x\left[\frac{1}{2}k\right]
$$

### Time inversion

Flipping the signal around the origins

$$
x\left[-k\right]
$$

## Combined Signal Operations

$$
x\left[-3k-15\right] = x\left[-3\left(k+5\right)\right]
$$

In this form, here is the order of operations.

$$
x\left[-\alpha\left(k+\beta\right)\right], \alpha > 0
$$

1. First scale by $$\alpha$$
2. Invert the scaled signal
3. Shift by $$\beta$$

Note that if the coefficient is not negative, you don't have to invert.
