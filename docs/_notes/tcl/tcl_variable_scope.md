---
tags: tcl, variables, guide, research
---

# Variable scope - global and upvar

Tcl evaluates variables within a scope delineated by procs, namespaces #TODO: Read on this, and at the topmost level, the `global` scope.

The scope in which a variable will be evaluated can be changed with the `global` and the `upvar` commands.

## `global`

`global` will cause the variable to refer to a global variable with the same name.

## `upvar`

`upvar` is a similar command. It will tie the name of a variable in the current scope with a variable in a different scope. This is how pass by reference is simulated to procs.

### Syntax

```tcl
upvar ?level? otherVar1 myVar1 ?otherVar2 myVar2? ... ?otherVarN myVarN?
```

This makes `myVar1` become a reference to `otherVar1` and the rest of the pairs do the same thing. The `otherVar` variable is declared to be at `level` relative to the current procedure. The default is level 1, which is just one layer up.

When you use an integer for `level`, then that references how many layers up the stack from the current level it will look.

If the `level` integer is preceded by a # symbol, then if references that many level down from the `global` scope. For example if `level` was set as `#0`, then it will be referencing the global scope. It is like the absolute leveling.



## `variable`

This is discussed in the namespace topic [here](tcl_namespace.md).

## `unset`

This command is used to manually apply garbage collection to the variable, but tcl actually already has automatic garbage collection, so it must be a very niche case for this to happen.

