---
title: "CS3110: 3.5 Advanced Pattern Matching"
topic: cs3110
header:
  teaser: /assets/images/ocaml_teaser.png
---

### Extra Pattern matching techniques

- `p1 | ... | pn` - This is the *or* pattern. Basically boolean or, which will be tried from left to right.
- `(p : t)` - Pattern with explicit type annotation.
- `c`: here, `c` means any constant. Integer literals, booleans, and string literals.
- 'ch1..chn' - Where `ch` is a character literal. For example, `A..Z` matches all uppercase characters.
- `p when e` matches `p` but only when `e` is `true`.

## 3.5.1 Pattern matching with `let`

The syntax for `let` is actually more expansive than just allowing a variable on the left-hand-side. The reality is that we are allowed to use patterns on that left-hand-side of the equality sign.

```OCaml
let p = e1 in e2
```

This changes the way we should think about the semantics of `let`.

### Dynamic Semantics

To evaluate `let p = e1 in e2`:
1. Evaluate `e1` to `v1`.
2. Match `v1` with `p`. If it doesn't then return a match error. If it does match, it will return a set of $$b$$ bindings.
3. Substituting those bindings into `e2` produces a new expression `e2'`.
4. Evaluate `e2'` into `v2`.
5. The result of evaluating the `let` expression is `v2`.

### Static Semantics

If the following holds, then `(let p = e1 in e2) : t2`:
- `e1 : t1`
- The pattern variable in `p` are `x1..xn`
- `e2 : t2` under the assumption that for all `i` in `1..n` it holds that `xi : ti`.

Since `let` definition are just a `let` expression that hasn't been given a body, it also holds that `let p = e` with pattern matching.

## 3.5.3 Pattern matching with functions

Surprise, the functions syntax given in the previous chapters are incomplete as well. The full function syntax can be seen here:

```OCaml
let f p1 ... pn = e1 in e2  (* Function as a part of let expression *)
let f p1 ... pn = e         (* Function definition *)
fun p1 ... pn -> e          (* Anonymous functions *)
```

Everything else with the semantics remain the same as before, please refer back to the previous chapter if you need clarifications on it.
