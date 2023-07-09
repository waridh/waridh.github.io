---
tags: tcl, guide, research, thread
---
# Threading in tcl

By default, tcl 8.4 is compiled without thread support, or script level access to the thread.

## Implementation Details

A tcl thread cannot share an interpreter, but there may be more than one interpreter per thread. An interpreter will act like a resource for the thread.

tcl threads are nearly as isolated as a process, but it does not have the fork/exec and address space switch overheads that you would get out of C.

They way that tcl works is that a process will contain at least one thread, and a thread will contain at least one interpreter. This is a little different from compiled languages as they do not have interpreters.

Interpreters may be safe or unsafe, and the master interpreter for each thread is always unsafe.

## Rules for threads

1. You can **NEVER** use the same interpreter from more than one thread.
2. If you only have one tcl interpreter:
    1. You can use either unthreaded or threaded tcl
    2. No *Big Global Mutex*. It is only required for Unthreaded tcl builds but never required for Threaded tcl builds.
3. If you have multiple tcl interpreter:
    1. If using unthreaded tcl *Big Global Mutex* is required for **ALL** calls to functions in libtcl
    2. If you are using Threaded tcl, then no mutex locks are required. WHAT????

### How to communicate between threads?

tcl scripts communicate by posting scripts onto the event queue of an interpreter in a different thread. This can be done either synchronously, or asynchronously.

In the 2.x version of the Thread Extension, there are shared variables, mutexes, and condition variables, so you have more objects typically used in threaded programming.

In the new 8.4 release, you can also transfer I/O channels between threads.