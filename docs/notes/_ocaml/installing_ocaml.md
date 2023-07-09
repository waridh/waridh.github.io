# Installing OCaml

## 1. Get UNIX environment

The first step in using OCaml is to get a UNIX environment working. In the context of Windows, this just means setting up WSL properly and running this command:

```Ubuntu
sudo apt install -y zip unzip build-essential
```

## 2. Installing OPAM

Run this command when using Ubuntu

```UNIX
sudo apt install opam
```

## 3. Initializing OPAM

```
opam init --bare -a -y
```

## 4. Creating an OPAM switch

```
opam switch create cs3110-2023sp ocaml-base-compiler.4.14.0
```

Every now and then, don't forget to run `opam update` when you cannot get a newer compiler.

Not sure what this one does, but `eval $(opam env)` is a command that is listed in the book. I used it, and I still don't know what it does. Moving on to restarting the OS.

To check if it works, you just need to input this command:

```
opam switch list
```

After you confirm that the switch is working, proceed to install the required package for it by doing the following:

```
opam install -y utop odoc ounit2 qcheck bisect_ppx menhir ocaml-lsp-server ocamlformat ocamlformat-rpc
```