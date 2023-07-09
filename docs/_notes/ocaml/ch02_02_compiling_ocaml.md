---
title: "CS3110 02.02 Compiling an OCaml Program"
layout: single
topic: cs3110
---

## Compilation

```bash
ocamlc -o hello.byte hello.ml
```

This will create the executable. Follow this up with:

```bash
./hello.byte
```

to execute the code. It works!

We can then manually clean up the files using the usual bash command.

```bash
rm hello.byte hello.cmi hello.cmo
```

## What about main?

Unlike lower-level language, OCaml does not need a main function to act as an entry point for the program. In OCaml, the last definition is what will be used to kick off the program.

## 2.2.3 Dune

In larger project, we don't want to be manually building our projects. OCaml has this build program called Dune that will do that for us. It's a similar system to `make` but since it's OCaml, this is better.

A Dune project is a directory and subdirectories that contain OCaml code that you want to compile. The root of the project is the highest directory in the hierarchy. Sometimes a piece of code will need precompiled code that is probably already installed through OPAM. All directory will contain a file named `dune` that tells Dune how we want to code the directory. There is a Dune manual that I can go and read if I want to further work with this build system in the future, which could very well happen.

Working with the `hello.ml` example from before, we are going to make a file named `dune` in the same directory and we are inputting:

```Dune
(executable
    (name hello))
```

So here, we declare an executable with the main file being `hello.ml`.

Follow up by creating a file called `dune-project`. This is required for all projects. In it, type in the following:

```dune
(lang dune 3.4)
```

Finally, in the terminal, type in this command:

```zsh
dune build hello.exe
```

The built executable will be a few layers down. The path will usually be the following:

```
_build/default/hello.exe
```

Now you could make this a two step process and run the program by doing `./_build/defualt/hello.exe` or you could use a built in Dune command:

```
dune exec ./hello.exe
```

which would build and run the executable using one command.

```
dune clean
```

`dune clean` will get rid of all the build clutter.
