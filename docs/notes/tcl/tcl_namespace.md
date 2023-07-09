# Packages and Namespace

We are utilizing packages in the tcl script for PVMaster, so it would be best to understand how it works. `source` allows you to separate a program into multiple files, but packages are a better way to handle modularity.

## Using packages

`package` command is how you use and manage packages. It also allows you to register your own package with an interpreter. A package is loaded by using the `package require` command and providing the package `name` and optionally the `version` number. The first time that a script requires a package, Tcl will build up a database of available packages and versions. It does some query work, but I think it is out of my scope for now.

Each package will provide a file called `pkgIndex.tcl` that tells Tcl the names and versions of any packages in that directory, and how to load them if they are needed. 

Good styling in Tcl to start every script with a set of `package require` statements, which makes sure that any missing requirements are identified as soon as possible; and, clearly documenting the dependencies of that code. Tcl and Tk are both made available as packages so you can explicitly call them, even if  they are already loaded in order to make the script more portable.

## Creating a Package

There are three steps:
1. Adding a `package provide` statement in the script.
2. Creating a `pkgIndex.tcl` file.
3. Installing the package where it can be found by Tcl.

*I am not adding the information regarding this right now, as I likely will not need it yet.*

## Namespaces

Created to deal with *name collisions* where multiple packages hold procs with the same name. The `namespace` command was made for dealing with this.

A namespace can contain commands and variables that are local to that namespace and cannot be overwritten by commands or variables in other namespaces. When a command in a namespace in invoked, it can see all the other commands and variables in its namespace, as well as the global namespace. A namespace can also contain other namespaces. It is like a hierarchy, somewhat like the rust module system. For example, to access the variable `bar` in the namespace `foo`, you would have to invoke `foo::bar`. That previous example is still only a relative pathing for namespaces. In order to write the absolute path, also known as a *fully-qualified* path, you you put another `::` at the front to represent global. It will look something like this: `::foo::bar`.

A namespace can also be exported to other namespaces, and it will show up like a local command or variable to that namespace.