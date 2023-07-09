# Global

## Synopsis

So the wiki page just has this listed on their **global** page:

```tcl
global varname ?varname...?
```

So when not in the body of a procedure, `global` declares a variable named `varname` in the current namespace, unless a variable by the same name exists in the global namespace, in which case it refers to that variable. This is likely a bug??? Using `variable` is supposed to be a better idea.

# Variable

It is good practice to use `variable` instead of `global` because `global` is broken. My god, the wiki for Tcl is very informal.

Here are some code example of how you would use `variable`:

```tcl
variable myvar1
variable myvar2
```

further readings on `variable` [here](tcl_variable.md).