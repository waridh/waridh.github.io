# `tell` and `seek`

In Tcl, you are able to open a file from a certain byte using seek as to start where you left off. `tell` will get you that byte offset. Let's look at it in more detail in this document.

## `tell`

Returns the current access position for an open channel. The return value is in the unit of bytes. If the type does not support `seek`, then it will return a -1.

### Syntax

```tcl
tell channelId
```

## `seek`

Changes the access position for an open channel.

### Syntax

```tcl
seek channelId offset ?origin?
```

### Description

Change the current access position for the channelId.

ChannelId must be an identifier for an open channel such as a Tcl standard channel (stdout, stdin, or stderr), the return value from an invocation of open or socket, or the result of a channel creation command provided by a Tcl extension. basically, has to be a Tcl file handle (fd).

#### Origin

Origin can be one of the following:
1. **start** - The new access position will be *offset* bytes from the start of the underlying file or device.
2. **current** - The new access position will be *offset* bytes from the current access position; a negative *offset* moves the access position backwards in the underlying file or device.
3. **end** - the new access position will be *offset* bytes from the end of the file or device. A negative *offset* places the access position before the end of file, and a positive *offset* places the access position after the end of file.

The default value of ?origin? is **start**.

