# 2.6 X's and Z's, Oh MY

Boolean algebra is limited to 0's and 1's, but real circuits can have illegal and floating values. This is represented symbolically with X and Z.

## 2.6.1 Illegal Value: X

X represent an illegal/unknown value. Usually occurs when the circuit is being driven to both 0 and 1 at the same time. This is called a contention, and it should be avoided. Usually it's between 0 and $V_{DD}$, but it's not always going to be in the forbidden zone depending on the pull up and pull down choices that were made.

What's nice is that some circuit simulators will notify you about cases where you have entered the forbidden zone with the symbol of X. Unfortunately, X also means don't care in digital design. These are not the same thing, so you would want to be really careful about this.

## 2.6.2 Floating Value: Z

Z means a floating value, which is the point in which the not is neither being driven high, or low. An undriven node is not the same as a 0. Some systems actually utilize floating signals in their data transfer, while others might treat it as an error.

Some ways to produce a floating value is to forget to connect a voltage to a circuit input. You actually do not know how the circuit will react if you do this, so it is best to be avoided.

![](../../../assets/Pasted%20image%2020230703155615.png)

The *tristate buffer*  actually has three possible output state. High (1), Low (0), and Floating (Z). The tristate buffer has an input A, output Y, and *enable* E. When the enable is true, then the tristate buffer will act as a normal buffer, but when the enable is false, then the output is allowed to float. It means that the node is not going to be used. There is an active high and active low tristate buffer The image above shows the symbol for an active high tristate buffer, while the picture below shows the active low enable.

![](../../../assets/Pasted%20image%2020230703160209.png)

Tristate buffers are usually used in busses that connect to multiple chips. If only one chip at a time is allows to connect to the bus, then the other chips must produce a floating output so that they do not cause contention with the active chip. This is a slow protocol however, and we have since moved on to point to point links.

