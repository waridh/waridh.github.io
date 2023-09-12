---
title: "ECE 301: Object-Oriented Design"
topic: cmput301

---

## Object-oriented thinking

In this class, there are a few major concepts which object-oriented design aids in achieving. They are as follows:

- Abstraction
- Encapsulation
- Decomposition
- Generalization

Let's go a little into each of them.

### Abstraction

Simplifying to its essential description of a real-world entity or concept based on the context of the problem.

- Helps with dealing with complexity. (Simplifies the design so that only the most relevant detail for the context is kept, while the irrelevant is ignored)
- Selective ignorance
- Modeling the problem space.

The abstraction of an object may change depending on the context, and it is good to keep track of which is the best abstraction for a certain problem.

### Encapsulation

Bundling data together, and controlling the access.

- Distinguishes the "what" from "how".
  - The "what" is outside the black box, and the "how" is inside the black box.
  - There is a "need to know" restricted access so that the implementer can control the interface.

We are trying to hide the implementation detail, since those can be changed, and we do not want users to make code that rely on some methods used to implement the actual interface.

### Decomposition

Divide the entire thing input parts/make the system out of parts.

- Fixed or dynamic number.
- Sharing of parts.
- Life-time of parts.

### Generalization

Looking for commonalities that can be factored out.

- Reusing common design
- Reduce redundant code

Basically, if you see repeated code, you should be factoring those out. This will keep the system flexible and extensible.

D.R.Y. - Don't repeat yourself.
