---
tags: tcl, guide
---
# TCL

Old scripting language that is still being used in many companies because they have a lot of legacy code built on it.

This document will have some notes about how things work for future reference

## Procedures

Instead of using functions in TCL, we instead have procedures. They can be defined using the following syntax.

```tcl

proc name {arg1 arg2}  {

    return [expr $a+$b]
}
```

to call the function, do this:

```tcl

puts [name 10 40]
```

## Command syntax

### Commenting

You cannot do in-line comments in TCL.

```tcl
# Do this
puts [expr 10+11] #Not this
```

### Obtaining the result of a command

You are able to obtain the result of a command by using `[]`. Here is an example of how that works.

### Grouping Operator

`{}` is the grouping operator in TCL. This will either group a string together like `{hello world}` or a list like `{a b c d e f}`. It can also group scripts together, like `{ puts -nonewline {hello } puts world\n}`.

## Data structures

### List

#### Initialization
You can make lists in multiple ways, here are some syntax for initialization:

```tcl
set listName {item1 item2 item3}

# Or

set listName [item1 item2 item3]

# Or

set listName [split "items separated by a character" " "]
```

#### Appending

There are two ways to append to a list in tcl:

```tcl
append listName split_character value

# Or

lappend listName value
```

In reality everything here is a string with some small nuances. Just know that.

#### Length of list

```tcl
llength listName
```

#### List item at index

Here is how you can retrieve the item at an index of a list

```tcl
lindex listname index
```

If you want to get the return from this index, you'd have to do the following:

```tcl
puts [lindex listName 2]
```

## Iteration

### Iterating over a list

The syntax for iterating over a list is foreach. Here is an example doing that.

```tcl
set values {1, 3, 5, 7, 8}
puts "Value\tSquare\tCube"
foreach x $values {
    puts " $x\t [expr {$x**2}]\t [expr {$x**3}]"
}
```

### Basic Iteration (For loop)

Similar to `C`, tcl does for loops the following way:

```tcl
# I think that it's weird that incr has to be done on i, and not $i
for {set i 0} {$i < 10} {incr i} {
    puts "In a loop right now. Itr: $i"
}
```

## Conditional

In programming you always find conditionals, so here is the syntax for this one:

```tcl
if { $a != $b } {
    # Do something
}
```

## Regular Expressions

### regex Rules and description

| Sr.No  | Rules and Descriptions |
| --- | --- |
| 1 | **`x`** Exact match |
| 2 | **`[a-z]`** Any lowercase letter from a-z |
| 3 | **`.`** Any character |
| 4 | **`^`** Beginning string should match |
| 5 | **`$`** Ending should match |
| 6 | **`\^`** Backlash sequence to match special character ^. You can do backslash sequence for other special characters |
| 7 | **`()`** Add the above sequences inside parenthesis to make a regular expression |
| 8 | **`x*`** Should match 0 or more occurrences of the preceding x. |
| 9 | **`x+`** Should match 1 or more occurrences of the preceding x. |
| 10 | **`[a-z]?`** Should match 0 or 1 occurrence of the preceding x. |
| 11 | **`{digit}`** Matches exactly digit occurrences of previous regex expression. Digit that contains 0-9.
| 12 | **`{digit,}`** Matches 3 or more digit occurrence of previous regex expression. Digit that only contains 0-9.
| 13 | **`{digit1,digit2}`** Occurrences matches the range between digit1 and digit2 occurrences of previous regex expression.

### TCL syntax

```tcl
regexp optionalSwitches patterns searchString fullMatch subMatch1 ... subMatchn
```

Just need to register this into my brain when working with regex, since that is super common in industry.

## Incrementing

### Syntax

```tcl
# For incrementing by one
incr x

# For incrementing by a certain number
incr x 52

# For incrementing by a variable
incr x $y
```
