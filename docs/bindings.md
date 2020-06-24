---
title: Bindings
---

A "let binding" _binds_ values to names. In other languages they might be called
a "variable declaration". They can be seen and referenced by code that comes
after them.

```reason
let greeting = "hello!";
let score = 10;
let newScore = 10 + score;
```

_Note: If you are coming from JavaScript, these bindings behave like `const`,
**not** like `var`/`let`._

## Bindings are Immutable

Reason let bindings are "immutable", they cannot change after they are created.

```reason
let x = 10;
/* Error: Invalid code! */
x = x + 13;
```

## Binding Shadowing

Bindings can be shadowed to give the appearance of updating them. This is a
common pattern and typically what should be used when it seems like a variable
needs to be updated.

```reason
let x = 10;
let x = x + 10;
let x = x + 3;
/* x is 23 */
```

## Block Scope

Bindings can be manually scoped using `{}`.

```reason
let message = {
  let part1 = "hello";
  let part2 = "world";
  part1 ++ " " ++ part2
};
/* `part1` and `part2` not accessible here! */
```

The last line of a block is implicitly returned.

## Mutable Bindings

If you really need a mutable binding then check out the [`ref` feature](mutable-bindings.md).
