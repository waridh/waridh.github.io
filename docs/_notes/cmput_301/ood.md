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

Here is the three key ideas laid out for encapsulation:

1. Bundling
  a. The attributes and behaviours are bundled into a single location.
2. Expose
  a. The interface in which the system outside the black box can interact with it. Some attributes and functions.
3. Restrict
  a. Restricting the access of certain functions and attributes that the users outside the black box does not need to concern themselves with. Not the purpose of the class, and may change with updates to the implementation.

We are trying to hide the implementation detail, since those can be changed, and we do not want users to make code that rely on some methods used to implement the actual interface.

The object being encapsulated also should not contain data that is not relevant to the object itself.

#### Java

The way that encapsulation can be represented in Java is via classes and the private and public members (protected as well).

### Decomposition

Divide the entire thing input parts/make the system out of parts. This is still separate from generalization, but together, they create an important concept.

- Fixed or dynamic number.
  - Is the number of parts a fixed number that never changes, or is it a dynamic number that changes over time?
- Sharing of parts.
  - A part can be shared between multiple wholes.
- Life-time of parts.
  - Is the part's lifetime closely tied to the lifetime of the whole, or is it more independent?

#### Java and UML

In UML, which can then model Java, decomposition is split into three types,

- Association
  - The two objects has some relationship. They are very loosely connected, and are mostly independent.
  - In Java, you would see objects with these type of relationship to just be passed as a parameter into a method in the other object.
- Aggregation
  - This is a has-a relationship. One object acts as a container for the other, but it is still a weak relationship. This means that both objects can exist independently of one another.
  - In Java, this is just an array member holding objects.
- Composition
  - Another has-a relationship, but this time, it is a strong relationship. The container cannot exist without the component, and likely vice versa.
  - In Java, you would denote this by instantiating the component object in the constructor of the whole object.

### Generalization

Looking for commonalities that can be factored out.

- Reusing common design
- Reduce redundant code

Basically, if you see repeated code, you should be factoring those out. This will keep the system flexible and extensible.

Model the system so that you reduce the amount of repetition. A part of automation, really.

D.R.Y. - Don't repeat yourself.

Together with *decomposition*, you achieve modularity.

Generalization exists in both OOP and functional programming.

- In functional programming, you will see this concept appear through higher-order functions.
- In object-oriented programming, you will see this concept through inheritance.
  - In inheritance, there may be many classes that share methods and attributes. This paradigm encourages the user make a super-class containing the shared methods, while leaving the specifics in the subclass.
