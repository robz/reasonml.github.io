---
title: Recursion
---

There are three places recursion is commonly used: bindings, types, and modules.

Feature  | Recursive           | Non-recursive
---------|---------------------|--------------
Bindings | `let rec fn = ...`  | `let fn = ...`
Types    | `type t = ...`      | `type nonrec t = ...`
Modules  | `module rec A ...`  | `module A ...`

## Recursive Bindings

By default the name of a binding is not available for use on the right side of
that binding. This applies to all `let` bindings, including function
definitions. This behavior is required for
[Binding Shadowing](bindings.md#binding-shadowing) to work:

```reason
let x = 10;
/* If bindings were recursive by default this would form a cycle and break */
let x = x + 10;
```

The natural way to write a recursive function will have an error:

```reason
let infiniteRecursion = () => {
  /* Error: Unbound value infiniteRecursion */
  infiniteRecursion();
};
```

Instead you have to manually opt-in to a recursive binding using the `rec`
keyword:

```reason
let rec infiniteRecursion = () => {
  infiniteRecursion();
};
```

## Mutual Recursion

To write mutually recursive functions use the `and` keyword:

```reason
let rec function1 = () => {
  function2();
}
and function2 = () => {
  function3();
}
and function3 = () => {
  function1();
};
```

## Recursive Types

Types, unlike bindings, are recursive by default. This is because it's fairly
common for types to be recursive and very uncommon for types to be overriden. A
normal recursive type could be used to define a binary tree:

```reason
type tree =
  | Leaf
  | Node(tree, tree);
```

To opt-out of recursive types use the `nonrec` keyword:

```reason
type t = string;
type nonrec t = list(t);

/* t is now list(string) */
let x: t = ["hello", "world"];
```

## Mutually Recursive Types

Types can be mutually recursive just like bindings using the `and` keyword:

```reason
type node = {
  value: string,
  edges: list(edge),
}
and edge = {
  weight: int,
  next: node,
};
```

## Mutually Recursive Modules

Sometimes functions will be organized across modules and both modules need to
be able to call functions from each other. In order to achieve this kind of
mutual recursion use the `module rec` and `and` keywords.

Note: Recursive modules require an explicit module type annotation.

```reason
module type X = {
  let x: unit => int;
  let y: unit => int;
};

module rec A: X = {
  let x = () => 1;
  let y = () => B.y() + 1;
}
and B: X = {
  let x = () => A.x() + 2;
  let y = () => 2;
};
```
