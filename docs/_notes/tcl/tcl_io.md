# Tcl I/O

There are many ways to read and write from a file in Tcl. `gets` and `puts` are simple ways to access a file, and `read` is a way to read the entire file at once, with `split` commonly used to parse the input file into lines.

## `open`

```tcl
open fileName ?access? ?permission?
```

`open` returns the file descriptor for the file that is being opened. `?access?` is the access mode for the file:
- `r`
- `r+`
- `w`
- `w+`
- `a`
- `a+`

## `close`

```tcl
close fileID
```

Closes the previously opened file, and flush any remaining outputs.

## `gets`

```tcl
gets fileID ?varName?
```

Reads a line of input from `fileID` and discards the terminating newline. When there is a `varName` argument, then `gets` will return the number of characters read (or -1 at EOF) and places the line of input in `varName`.

If `varName` is not specified, then `gets` return the line of input. An empty string will return if:

- There is a blank line in the file
- The current location is at the end of the file. (EOF occurs.)