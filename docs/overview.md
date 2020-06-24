---
title: Overview
---

This is an overview of most language features in Reason. It does not explain
them in detail, but should serve as a quick reference. Please see the guides
on the left for additional details about each feature.

## Standard types

Feature                         | Example
--------------------------------|----------
Int                             | `let x: int = 10;`
Float                           | `let x: float = 10.0;`
Boolean                         | `let x: bool = false;`
String                          | `let x: string = "ten";`
Char                            | `let x: char = 'c';`
Unit                            | `let x: unit = ();`
Option                          | `let x: option(int) = Some(10);`
Tuple                           | `let x: (int, string) = (10, "ten");`
List                            | `let x: list(int) = [1, 2, 3];`
Array                           | <code>let x: array(int) = [&#124;1, 2, 3&#124;];</code>
Functions                       | `let x: (int, int) => int = (a, b) => a + b;`

## Strings

Feature                         | Example
--------------------------------|----------
String                          | `"Hello"`
String concatenation            | `"Hello " ++ "World"`
Character                       | `'x'`
Character at index              | `let x = "Hello"; x.[2];`

- String Functions: [`module String`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/String.html)

## Numbers

Feature                         | Example
--------------------------------|----------
Integer                         | `23`, `-23`
Integer operations              | `23 + 1 - 7 * 2 / 5`
Float                           | `23.0`, `-23.0`
Float operations                | `23.0 +. 1.0 -. 7.0 *. 2.0 /. 5.0`
Float exponentiation            | `2.0 ** 3.0`

## Logical operators

Feature                         | Example
--------------------------------|----------
Comparison                      | `>`, `<`, `>=`, `<=`
Boolean operations              | `!`, `&&`, <code>&#124;&#124;</code>
Reference equality              | `===`, `!==`
Structural equality             | `==`, `!=`
If-Else expressions             | `if (condition) { a; } else { b; }`
Ternary expressions             | `condition ? a : b;`

## Functions

Feature                         | Example
--------------------------------|----------
Function definition             | `let divide = (a, b) => a / b;`
Function calls                  | `divide(6, 2); // 3`
Named arguments                 | `let divide = (~a, ~b) => a / b;`
Calling named arguments         | `divide(~a=6, ~b=2); // 3`
Recursive functions             | `let rec infinite = () => infinite();`

## Advanced Functions

Feature                         | Example
--------------------------------|----------
Function currying               | `let divideTen = divide(10); divideTen(5); // 2`
Currying specific args          | `let half = divide(_, 2); half(10); // 5`
Default arguments               | `let divide = (~a=100, ~b) => a / b;`
Optional arguments              | `let print = (~prefix=?, text) => {...};`
Function chaining (pipe)        | <code>32 &#124;> half &#124;> half; // 8</code>

## Function Types

Feature                         | Example
--------------------------------|----------
Inline typing                   | `let divide = (a: int, b: int): int => a / b;`
Standalone type                 | `type fn = (int, int) => int;`
Typing optional arguments       | `let print = (~prefix: option(string)=?, text) => {...};`


## Basic Structures

Feature                         | Example
--------------------------------|----------
List (Immutable)                | `[1, 2, 3]`
List add to front               | `[a1, a2, ...theRest]`
List concat                     | `[a1, a2] @ theRest`
Array (Mutable)                 | <code>[&#124;1, 2, 3&#124;]</code>
Array access                    | <code>let arr = [&#124;1, 2, 3&#124;]; arr[1];</code>

- List Functions: [`module List`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/List.html)
- Array Functions: [`module Array`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/Array.html)

## Maps and Sets

There are several different ways to interact with Maps and Sets depending on the
specific environment being used. In standard Reason code there are `Map` and
`Set` modules:

- [`module Map.Make`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/Map.Make.html)
- [`module Set.Make`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/Set.Make.html)

When using BuckleScript `belt` exposes these modules:

- [`module Belt.Map`](https://bucklescript.github.io/bucklescript/api/Belt.Map.html)
- [`module Belt.Set`](https://bucklescript.github.io/bucklescript/api/Belt.Set.html)

There are also other libraries that will provide their own implementation of
these data structures. Check the style guide of the project you are
working in to determine which module to use.

## Records

_Details: [Records](records.md)_

Feature                         | Example
--------------------------------|----------
Record definition               | `type t = {foo: int, bar: string};`
Record creation                 | `let x = {foo: 10, bar: "hello"};`
Record access                   | `x.foo;`
Record spread                   | `let y = {...x, bar: "world"};`
Destructuring                   | `let {foo, bar} = x;`
Mutable record fields           | `type t = {mutable baz: int}; let z = {baz: 10};`
Mutable record updates          | `z.baz = 23;`
Polymorphic records             | `type t('a) = {foo: 'a, bar: string};`

- Note: Record types are [nominal](https://en.wikipedia.org/wiki/Nominal_type_system#:~:text=Nominal%20typing%20means%20that%20two,declarations%20name%20the%20same%20type.). This means that two different record definitions (`type x = {...};`) with the exact same fields are not compatible. They cannot be used interchangeably and cannot be spread into each other.

## Variants

Variants are used to represent different variations of something. This feature
is sometimes called disjoint unions or tagged unions in other languages.

Feature                         | Example
--------------------------------|----------
Variant definition              | <code>type t = &#124; Foo &#124; Bar;</code>
Variants with args              | <code>type t = &#124; Foo(string) &#124; Bar(int);</code>
Polymorphic variants            | <code>type t('a) = &#124; Foo('a) &#124; Bar(int);</code>
Using a variant                 | `let x = Foo("Hello");`

## Options

Options are an important variant that represent the presence or absence of a
value. In other languages this can be thought of like null values. Options are
used often and are important to learn.

Feature                         | Example
--------------------------------|----------
Definition (already defined)    | <code>type option('a) = &#124; None &#124; Some('a);</code>
Value that is present           | `let x = Some("Hello World");`
Value that is absent            | `let y = None;`

## Pattern Matching

Pattern matching is a very powerful feature in Reason. It matches against variants
and ensures all cases are covered. Start matching using the `switch` keyword:

```re
switch (foo) {
| Some(value) => doSomething(value)
| None => error()
}
```

Feature                         | Example
--------------------------------|----------
Basic case                      | <code>&#124; Some(value) => doSomething(value)</code>
When conditions                 | <code>&#124; Some(value) when value > 10 => doSomething(value)</code>
Catch-all case                  | <code>&#124; _ => doSomething()</code>
Matching lists                  | <code>&#124; [a, b, ...rest] => doSomething(rest)</code>
Matching records                | <code>&#124; {foo: value} => doSomething(value)</code>
Matching literals               | <code>&#124; "Hello" => handleHello()</code>

## Unit

Units are typically used for side-effects. They represent something that never
has any meaningful value (this is distinct from options which may have a value).

Feature                         | Example
--------------------------------|----------
Creating a unit                 | `let x = ();`
Passing to a function           | `fn(a, b, ());`
Unit as only argument           | `let fn = () => 1; fn();`


## Refs

_Details: [Mutable Bindings](mutable-bindings.md)_

Refs are a way to have a mutable "variable" in your program. It is a thin wrapper
around a record with a mutable field called `contents`.

Feature                         | Example
--------------------------------|----------
Type (already defined)          | `type ref('a) = {mutable contents: 'a};`
Ref creation                    | `let x = ref(10);` or `let x = {contents: 10};`
Ref access                      | `x^;` or `x.contents;`
Ref update                      | `x := 20;` or `x.contents = 20;`

## Loops

Loops are discouraged in most cases. Instead functional programming patterns
like `map`, `filter`, or `reduce` can usually be used in their place.

Feature                         | Example
--------------------------------|----------
While                           | `while (condition) {...}`
For (incrementing)              | `for (i in 0 to 9) {...}` (inclusive)
For (decrementing)              | `for (i in 9 downto 0) {...}` (inclusive)

- Note: There is no `break` or early returns in Reason. Use a ref containing a
bool for break-like behavior: `let break = ref(false); while (!break^ && condition) {...};`

## Modules

Modules are a way to group things. Each Reason file implicitly creates a module
of the same name. Modules can be nested.

Feature                         | Example
--------------------------------|----------
Module creation                 | `module Foo = { let bar = 10; };`
Module access                   | `Foo.bar;`
Module types                    | `module type Foo = { let bar: int; };`

## Functors

Functors are like functions that create modules. This is an advanced topic
that can be very powerful. Here is a basic example:

```reason
module type Stringable = {
  type t;
  let toString: (t) => string;
};

module Printer = (Item: Stringable) => {
  let print = (t: Item.t) => {
    print_endline(Item.toString(t));
  };

  let printList = (list: list(Item.t)) => {
    list
    |> List.map(Item.toString)
    |> String.concat(", ")
    |> print_endline;
  };
};

module IntPrinter = Printer({
  type t = int;
  let toString = string_of_int;
});

IntPrinter.print(10); // 10
IntPrinter.printList([1, 2, 3]); // 1, 2, 3
```

## Comments

Feature                         | Example
--------------------------------|----------
Multiline Comment               | `/* Comment here */`
Single line Comment             | `// Comment here`
