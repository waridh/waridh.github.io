# Designing Architecture

The dos and don'ts of making system architecture. It happens that this is my side project during my co-op, so I think that it should be an important topic to cover.

## Things to look for when designing software

### 1. Have a clear understanding of the requirements

When working on a project, you should have a good idea of what the functional and non-functional requirements are. This will let us create a final product that people are satisfied with.

We need to understand that requirements before starting so that we do not get lost or waste resources.

Visualization can help with this stage of development.

- **Begin with a high level view:** You need to sketch out the requirements as a mind map.
- **Map the functional requirements:** Like how objects must be able to interact with each other.
- **Consider non-functional requirements:** While working with the mind map, you should not focus too hard on things that are not functional. Things like performance can be optimized a little later.

The requirements should be used for scoping the work, and also planning out the project.

### 2. Start thinking about each component

The requirements might have already forced some technology that are going to be used for a project. Before going into implementation, we should think about which requirement will pose the biggest challenge to your design/project plan.

- **Start with a perfect world:** What would the design look like if it was created perfectly with no constraints?
- **Consider and document the implications of requirements:** Start a working draft, and develop it gradually.
    - Start with what the requirement imply for the design. Look at the wish list from the stakeholder may contradict each other or not. Is it in conflict with the functional and non-functional requirements?
- **Wait and design the final architecture later:** In the process, the architecture will likely change, so the first draft would not need to resemble the final result.

### 3. Divide your architecture into slices

In the planning phase of the architecture, we figure out how we are going to implement the design. Dividing the architecture into slices can help you craft the plan that will deliver results and also effectively use development resources.

You can either slice the architecture in terms of individual features (vertical slicing) but include all the tech layer, or you can choose to look at horizontal slices, which separate the tech layer (front-end, database, backend, api).

### 4. Prototype

Prototyping in software works well since we have infinite free retries. This can get you quick feedbacks, and also get you to your proof-of-concept faster. This is important in validation of the work done.

Be somewhat alright with some imperfections -> the first few prototypes are likely to be a little broken, and the final ones will still have some sort of improvements that can be done. 

Take advantage of this stage because there are no substitute for testing.

1. **Keep a careful revision history:** You need to keep track and document the things that you learn. Also document the design decision and why it was chosen. This is so that the whole team can keep up with designs and why things were done.
2. **Have a single source of truth:** Keep a single source for the documentation. We don't want experiments to side-track and break the focus of the project.
3. **Diagram the project:** Use diagrams to keep track of the prototyped changes and differences between each versions.

### 5. Identify and quantify non-functional requirements

Take non-functional requirements into account. They are as important as your functional requirements, it's just harder to implement immediately. They define the system's characteristics.

Non-functional requirements could be placed on either the whole project or just a slice of the architecture. You need to make sure to inform your stakeholders about them when they come up.

Even though these are usually qualitative requirements, you must quantify it. You can't just say you want performance, but not say when it is enough, as you could always improve performance.

Here are some common non-functional requirements that you would see:
- **Performance:** How well the entire system runs.
- **Scalability:** Current and future potential scaling on the system.
- **Portability:** How portable is the system, from perspective of the entire system, and just components.
- **Extensibility:** How well can the system handle future required adaptation, and how hard would it be to do so.
- **Compliance** No notes on this one.

Diagramming this might be useful for good planning as well. As with functional requirements, you should also consider how much effort it takes to meet each non-functional requirements.

## Best Practice

Some best practices to keep you following good design principals:

### Visualize your design

Lets your team members see the high level perspective behind the design. Diagrams are a good way to visualize the process and different aspects of the design choices.

### Don't choose patterns

Do not start a project planning to use Object Oriented Programming. It might be very inefficient. Instead, start from the high level goals, and work down so that you don't over engineer the solution.

### Remember that the first design is only the first iteration

The first design is really more like a tool to get a better idea of the challenges in the project and the obstacles that you will face in the architecture development.

The design will grow and develop as more work is being put into it.

### Careful of Scope Creep

You will be tempted to allow the scope to grow, but don't let that beast get fed too much. It might thin out resources needed for the project. It is very important to get the planning done earlier so that you start a project that doesn't get finished well.

### Keep boundaries and interfaces in mind.

When planning the design, think about interfaces between components and note the boundaries of the system.

## Citation

- [LucidChart guide](https://www/lucidchart.com/blog/how-to-design-software-architecture)