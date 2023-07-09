# Importing in tcl

This information is prerequisite to working with global variables in tcl.

## Source

```tcl
source fileName
```

for example, you would type in:

```tcl

source foo.tcl
source bar.tcl
```

To import files that you have created. The return from this source is the return value from the script that was sourced from. If there was an error executing, then it would return that error.

Specifically, the source command will read the script up to the tcl end of file character.

## Package

### Description

From what I can gather, package require acts like import for a library in other programming languages. 

### Package provide

This command is usually invoked as a part of the ifneeded script, and then again by the package when it gets loaded. There will be an error if a different version of the *package* has been provided by a previous package provide command. If the version argument is omtted, then the command returns the version number that is currently provided, or an empty string if no package provide command has been invoked for package in this interpreter.

### Package require

Evaluates a script when a certain package is required and then ensures that the package is present.

#### Description

Package require evaluates a script that must provide the required package. If a specific version is provided, then only the matching version is provided, or it will return an error.

There are two flags that are used with this, `-exact` and `version`. Version is used to specify the version required, but without the `-exact` flag also there, the minor version could be equal or greater than what was specified. Else the `-exact` flag requires that the exact version that was specified is returned.