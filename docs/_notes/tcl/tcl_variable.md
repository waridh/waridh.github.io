# Variable

```tcl
variable ?name value...? name ?value?
```

Normally used within a **namespace** eval command to create one or more variable within a namespace. Each variable *name* is initialized with a *value*. The *value* for the last variable is optional.

If the variable name doesn't already exist, then it will be created. In this case, if *value* is specified, it is assigned to the newly created variable, else the new value will be left undefined. Normally the variable will be created in the current namespace, but if you provide the absolute namespace pathing, then it will created in that specified path.
