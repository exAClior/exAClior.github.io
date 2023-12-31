+++
title = "OCaml Learning"
author = ["Yusheng Zhao"]
date = 2023-12-30T00:00:00+08:00
tags = ["PL", "OCaml"]
draft = false
+++

## What's OCaml {#what-s-ocaml}

OCaml is a **functional programming language**. I intend to use it for recreating
the `Pie` programming language in the book **the Little Typer**. This post will
serve as a learning note for OCaml based on the course [CS3110](https://www.cs.cornell.edu/courses/cs3110/2020sp/textbook/).


### Meaning of Name {#meaning-of-name}

It means **Objective categorical abstract machine langauge**. ML part in the name
originates from the famous `Meta Language` which is created for making a proof
assistant.


## Why OCaml {#why-ocaml}

-   Immutability of `variables` means easier debugging
-   More abstraction?
-   Better type system?
-   More exposure to theory and implementation of programming languages.

Compare to Haskell, OCaml has a good mix of functional and imperative
programming feature. This provides a smooth transition into the functional side
of programming from an imperative programming perspective while still giving you
some new materials to chew onto.

Functional programming has brought many features into imperative programming
like garbage collection, generics, type inferences etc. Therefore, it's
desriable to have some concepts in functional programming in order to improve
your programming capabilities in general.


## History {#history}

At the very beginning, there was a language called `ML`. It was created by Robin
Milner and others in the 1970s. It was created as a language for writing proof
assistants. `OCaml` is a dialect of `ML` created by the French computer
scientist Xavier Leroy and collaborators.


## What is a functional programming language {#what-is-a-functional-programming-language}

In a functional programming language, a computation is expressed as mathematical
functions. It avoids mutable state. The other end of the spectrum is imperative
programming languages, which breaks **referential transparency**. Meaning,
assignment has different meaning than equality. For example, if at time \\(t\\) in
your program, you execute `y = f(x)`, it does not mean you could substitue for
all later the appearance of `y` with `f(x)`. The value of `x` or even `f(x)` may
change and cause your entire program to give different result.


## Features of OCaml {#features-of-ocaml}


### Types {#types}

Ocaml is a **statically-typed** and **type-safe** programming language. Type errors
are detected at compile time. Such guards against type errors means the language
is type safe. This reduces the possibility of a buffer overflow error and
related exploitation. (?)


## What to learn for a new language {#what-to-learn-for-a-new-language}

When approaching a new language, there are a few concepts that you should learn
about it.

-   Syntax: how you write language, mostly facts. For example, keywords, white
    spaces (python), operators and formattings.
-   Sematics: What do programs mean? Includes dynamics sematics and static
    sematics. For example, type checking is static sematics. Static here means the
    programming is not running but the compiler is. Dynamics sematics takes in
    expression in a programming language to produce a value, exception or infinite
    loop.
-   Idioms: typical patterns used in a language to express ideas
-   Libraries
-   Tools: repls, debuggers, or GUI editors


## Basics {#basics}

OCaml comes with a repl called `utop`. To start it, simply type `utop` in the
terminal. To exit, type `#quit`.

There are a few basic types in OCaml, `int`, `float`, `bool`, `char`, `string`.

```ocaml
31 * 33;;
(* the double semi-colon is required in utop but not allowed in regualr scripts *)
```

The output of `utop` is `- : int = 1023`. The `-` means the value has no name.
The `int` means the type of the value is `int`. The `1023` is the value.

The operators for float and integers are different, the float ones requires a
`.` after the regualr operators

```ocaml
31.0 *. 33.0;;
```


### Type Annotation {#type-annotation}

Although OCaml is a statically typed language, it is not necessary to annotate
each expression with its type. However, it's possible to do type annotation to
put further restriction for debugging purposes. For example

```ocaml
(31 * 11 : int);;
```

Note, the `()` around the value is necessary.


### Let definition {#let-definition}

A **definition** in OCaml is distinct from **expression** or **values**. It does not have a value, hence you cannot combine it with the other expressions to form more complicated things.

```ocaml
(let z = 10) + 22;;
```

You need an **identifiers** , which needs to start with lower case character and
also an expression that can be evaluated to give the value to the identifier.

```ocaml
let x = 10 + 22;;
```

Note, you can also use `let` to define functions

```ocaml
let add x y = x + y;;
```

Note, you could load scripte you write within `utop` with directives like `#use`

```ocaml
#use "resources/mycode.ml";;
inc 3;;
```

There's a recommended workflow in `utop` for development.

1.  Edit code in file
2.  Load code in `utop` and test it
3.  Exit `utop` to have fresh code loaded next time you test it.

Unfortunately, the third step cannot be skipped. Reminds us how great [Revise.jl](https://github.com/timholy/Revise.jl)
was.


#### Chained let expression {#chained-let-expression}

We could chain `let` expressions, this is like saying we have a large expression
and we gradually substitute values into identifiers in the expression. Note,
OCaml likes `snake_case` naming style not the `camelCase`.

```ocaml
let a = "hello" in
let b = "world" in a^ b;;
```


#### Scope defined by let {#scope-defined-by-let}

`let` tells the program what **value** to substitute for a certain **identifier**.
The scope of validity of this substitution is defined to be at the inner most
`let` where we have

```ocaml
let x = 1 in
let x = 2 in
x;;
```


### Compiling {#compiling}

The format was somewhat similar to that of C. Say you have a file `hello.ml` and
you want to compile it, you could do

```sh
ocamlc -o hello.byte hello.ml
```

If you would like to automate these, you could use `dune`, `OCaml`'s own build
system like `Makefile`. You will need a `dune` file in your project's toplevel
directory.

Your build will be in folder `_build`.


### Expressions {#expressions}

Some reference are

1.  [OCaml Expressions](https://v2.ocaml.org/manual/expr.html)
2.  [OCaml Values](https://v2.ocaml.org/manual/values.html)


#### Int {#int}

OCaml `int` are always \\(64\\) bit in memory but I lacks \\(1\\) bit due to OCaml
implementation. This is due to the other types usually have `header` plus `data`
implementation. The `header` is a \\(64\\) bit number. That is word-aligned (the
details of which I still don't understand but it's guaranteed to have last few
bits being zero). For `int` type value, it's no longer stored as `header` and
`data`. An int `x` is stored as `(x << 1) | 1`. Meaning the last bit of an int
will always be \\(1\\). Therefore, there's an easy way to tell the type of data by
simply checking the last bit of the `header` of a value.(We are treating the
actual data of int like its header now). See [this](https://blog.janestreet.com/what-is-gained-and-lost-with-63-bit-integers/) Jane stree blog for details.


#### Float {#float}

As spoken before you need special operator for operating on `floats`. OCaml does
not support operator overloading.

To convert between `float` and `int`, use function `float_of_int`.

```ocaml
let x = 2;;
3.14 *. (float_of_int x);;
```


#### bool {#bool}

```ocaml
let x = true;;
let y = false;;
x || y;;
x && y;;
```


#### char {#char}

Represented as \\(8\\) -bit integers. Conversion available by functions
`char_of_int` and `int_of_char`.


#### string {#string}

Concatenation via `"abc"^"def"`. Conversion available by `string_of_int` and
`string_of_bool`. Need `String.make 1 'z'` to convert char to string. Indexing
is possible. It's \\(0\\) based.


### <span class="org-todo todo TODO">TODO</span> Equality Operators {#equality-operators}

Explain the difference between `<>` vs `!=` and `=` vs `==`.


### Assertions {#assertions}

```ocaml
assert (1 = 2);;
```


### If else {#if-else}

```ocaml
if "batman" > "superman" then "joker!" else "Luther!"
```

Note the returned expression needs to have the same type

```ocaml
if "batman" > "superman" then "joker!" else 1
```

Nesting of the `if else if` is possible

```ocaml
if "batman" > "joker" then "Earthling survives!"
else if "superman" > "Luther" then "Earthling survives"
else "We are doomed!"
```


#### Connect to Dynamics and Static Sematic {#connect-to-dynamics-and-static-sematic}

To connect back to previous definition of dynamic and static sematics, we examin
the following `if e1:t1 then e2:t23 else e3:t23`. we say at compile time the
type of this entire expression is `t23`, it's inferred as a static sematics. At
the same time, dynamic sematics evaluates the value of this expression to either
the value of `e2` or `e3` depending on the runtime value of `e1` being true or
false.


### Functions {#functions}

There are two ways to define a function in OCaml, they are syntactically
different but semantically the same.

```ocaml
let inc = fun x -> x + 1;;
let inc x = x + 1;;
(* the later is syntactic sugar of the previous  *)
```


#### Recursive function {#recursive-function}

You need to explicitly indicate this function is recursive by the `rec` keyword.

```ocaml
let rec fact n =
  if n = 0 then 1
    else n * fact (n-1);;
fact 10
```

Note, you could also denote mutually recursive function with `and` keyword

```ocaml
(** [even n] is whether [n] is even.
    Requires: [n >= 0]. *)
let rec even n =
  n = 0 || odd (n - 1)

(** [odd n] is whether [n] is odd.
    Requires: [n >= 0]. *)
and odd n =
  n <> 0 && even (n - 1);;

even 10;;
```


#### Lambda Expression: anonymous functions {#lambda-expression-anonymous-functions}

Note, lambda expression by itself in its definition is already a value, no
computation needs to be done.

```ocaml
(fun x -> x + 1) 2;;
```


#### Pipeline {#pipeline}

There's `@@` which treats the entire right side of it as an expression and pipes
it to the left hand side function. This avoids the need to write ugly parenthesis.

```ocaml
let square x = x * x;;
square @@ inc 5;;
```

There's also the pipleline operator we know and love in Julia `|>`. This creates
a convinent way of chaining multiple application of functions.

```ocaml
5 |> inc |> square;;
```


### Polymorphic {#polymorphic}

In OCaml, you denote an type variable, a variable holding the type of a variable
with `'` and a name. I.e `'a`.

```ocaml
let id x = x;;
```

Notice, we could instantiate a specific type version of the polymorphic function
by explicitly specifying the type information.

```ocaml
let first x y = x;;
let first_int : int -> 'b -> int = first;;
(* first_int 10 20;; *)
(* let bad_first : int -> 'b -> string = first;; *)
```

Note, the `bad_first` doesn't work because the type are inconsistent.


### Labeled and Optional Arguments {#labeled-and-optional-arguments}

It looks like keyword and optional arguments

```ocaml
let f ~name1:(arg1 : int) ~name2:(arg2 : int) = arg1 + arg2;;
let f ?name:(arg1=8) arg2 = arg1 + arg2;;
```


### Partial Application {#partial-application}

All multi-argument functions in OCaml are just partial application functions
applied in chain.

```ocaml
let addx x = fun y -> x + y;;
addx 2 @@ 3 ;;
```


### Operators as function {#operators-as-function}

You could define your own infix operators.

```ocaml
let (<^>) x y = max x y;;
10 <^> 20;;
```


### Tail Recursion {#tail-recursion}

Recursive call eats up your stack memory. It achieves so by reusing the stack
memory of the previous caller. But the programmer needs to rewrite the program
such that the compiler knows nothing needs to be done in the caller anymore but
return the returned value of next level function.

```ocaml
let rec count n =
  if n = 0 then 0 else 1 + count (n - 1);;

let rec count_aux n acc =
  if n = 0 then acc else count_aux (n - 1) (acc + 1);;

let count_tr n = count_aux n 0;;

(* this will explode your stack  *)
(* count 100000;; *)

(* the following will not *)
count_tr 10000000;;
```


### The None {#the-none}

The usual `None` return value is denoted as `()` in OCaml, you will see it being
displayed as `unit` which only has one value.

```ocaml
let () = print_endline "Camels" in
let () = print_endline "Horses" in
print_endline "Donkeys";;
```


### Semicolon {#semicolon}

Notice in the above statement, it's a hassel to have to write `let _ in` each
time. We are **not** binding any value to an expression, so we could replace it
with `;` like below

```ocaml
print_endline "Camels";
print_endline "Horses";
print_endline "Donkeys"
```


### Ignoring output {#ignoring-output}

Due to type safety requirements, if we truly want to output a `unit` type no
matter the output type of our computation, we could use the ignore function.

```ocaml
ignore @@ 2 + 3;;
```


### Debugging {#debugging}

You could either employ printing or tracing for debugging. To enable tracing use
directive `#trace`.

```ocaml
let rec fib x = if x <= 1 then 1 else fib (x - 1) + fib (x - 2);;
#trace fib;;
fib 3;;
```


### Defensive Programming {#defensive-programming}

Don't assume users know the range of arguments, give tests to make sure they
input correct range of arguments.

```ocaml
(* possibility 1 *)
let random_int bound =
  assert (bound > 0 && bound < 1 lsl 30);
  (* proceed with the implementation of the function *)

(* possibility 2 *)
let random_int bound =
  if not (bound > 0 && bound < 1 lsl 30);
  then invalid_arg "bound";
  (* proceed with the implementation of the function *)

(* possibility 3 *)
let random_int bound =
  if not (bound > 0 && bound < 1 lsl 30)
  then failwith "bound";
  (* proceed with the implementation of the function *)
```


#### Type safety and Memory safety {#type-safety-and-memory-safety}

-   [What is memory safety](http://www.pl-enthusiast.net/2014/07/21/memory-safety/)
-   [What is type safety](http://www.pl-enthusiast.net/2014/08/05/type-safety/)


## Data and Types {#data-and-types}

There are a few familiar types in OCaml. In particular, there's `list` and
`tuple`. There's also `record` and `variant`. They correspond to `struct` and
`enum`.


### Lists {#lists}

-   Singly-linked
-   Immutable


## Compiler vs Interpreter {#compiler-vs-interpreter}

A compiler is a program that takes in a program written in a programming
language and outputs a program in another programming language. Neither the
input code or the compiler is needed to run the output program. The primiary job
of a compiler is to do **translation** so they are more performant.

On the other hand, an interpreter takes the code of a source program as input
and prepares itself to take input and give output. In this sense, the
interpreter is easier to implement.


### Just-in-time compilation {#just-in-time-compilation}

When an interpreter sees some code is being run over and over again, it can
improve performance by compiling this piece of code to byte code to improve
performance.


### Architecture {#architecture}

A compiler has two parts, a front end and a backend. The frontend is responsible
for translating the source code into **Abstract Syntax Tree** and then into
**Intermediate Representation**. The backend is then responsible for translating
the IR into machine code. The interpreter on the other hand does not have
backend. It executes **AST** or **IR** directly. (But how?)

The frontend of them follows three steps

1.  Lexer: it parses the source code characters into word streams, known as
    **tokens**.
2.  Parser: it constructs an **Abstract Syntax Tree** out of the word steam. These
    nodes are known as **AST nodes**.
3.  Sematics analysis: it analyzes the sematics of the AST, checking for type
    errors etc.


### Parsers and Lexers {#parsers-and-lexers}

You don't have to write your own parse and lexers as OCaml has those [included.](https://v2.ocaml.org/manual/lexyacc.html)


#### Lexers {#lexers}

A lexer is built as **deterministic finite automata**. Upon specifying the
**regular expression** that describes the **regular language** it expects, the lexer
will output a program that implements the automaton. This automaton then accepts
"source code" and lexizes them.

An OCaml lexer generator, `ocamllex`, generates `.ml` file from a `.mll` file.

<!--list-separator-->

-  Header

    The header of a `.mll` file we need to specify the type of the tokens. This is
    done by loading the `Parser` module, generate by our `.mly` file.

    ```text
    {
    open Parser
    }
    ```

<!--list-separator-->

-  Identifier

    The **regular expressions** are denoted as **identifiers**. They are defined by the
    following

    ```text
    let white = [' ' '\t']+
    let digit = ['0'-'9']
    let int = '-'? digit+
    let letter = ['a'-'z' 'A'-'Z']
    let id = letter+
    ```

<!--list-separator-->

-  Rules

    When we have defined the **identifiers**, we could then define the **rules** that
    specify the **regular language** the lexer expects and tell you what to do after.

    ```text
    rule read =
      parse
      | white { read lexbuf }
      | "true" { TRUE }
      | "false" { FALSE }
      | "<=" { LEQ }
      | "*" { TIMES }
      | "+" { PLUS }
      | "(" { LPAREN }
      | ")" { RPAREN }
      | "let" { LET }
      | "=" { EQUALS }
      | "in" { IN }
      | "if" { IF }
      | "then" { THEN }
      | "else" { ELSE }
      | id { ID (Lexing.lexeme lexbuf) }
      | int { INT (int_of_string (Lexing.lexeme lexbuf)) }
      | eof { EOF }
    ```


#### Parser {#parser}

A parser is built as **push-down automata**. Upon specifying the **context-free
language** it expects, the parser will output a program that implements the
automaton. This automaton then accepts "source code" and parses them according
to the **context-free grammer** into the **Backus-Naur Form**.

An OCaml parser generator `ocamlyacc` or `Menhir` that generates `.ml` file from a `.mly`
file. The `.mly` file consist of the following parts

<!--list-separator-->

-  Header

    The header of a `.mll` file looks like this. It's necessary to avoid having to
    write things like `Ast.Int i`. Instead, you could do `Int i`.

    ```text
    %{
      open Ast
    %}
    ```

<!--list-separator-->

-  Declaration

    ```text
    %token <int> INT
    %token <string> ID
    %token TRUE
    %token FALSE
    %token LEQ
    %token TIMES
    %token PLUS
    %token LPAREN
    %token RPAREN
    %token LET
    %token EQUALS
    %token IN
    %token IF
    %token THEN
    %token ELSE
    %token EOF
    ```

    The associativity of operators is specified by the following. The precedence is
    from low to high.

    ```text
    %nonassoc IN
    %nonassoc ELSE
    %left LEQ
    %left PLUS
    %left TIMES
    ```

<!--list-separator-->

-  Rules

    The rules are specified by the following. They resemble the BNF form of the
    language.

    ```text
    %start <Ast.expr> prog
    expr:
      | production  { action }
      | i = INT { Int i }
      | x = ID { Var x }
      | TRUE { Bool true }
      | FALSE { Bool false }
      | e1 = expr; LEQ; e2 = expr { Binop (Leq, e1, e2) }
      | e1 = expr; TIMES; e2 = expr { Binop (Mult, e1, e2) }
      | e1 = expr; PLUS; e2 = expr { Binop (Add, e1, e2) }
      | LET; x = ID; EQUALS; e1 = expr; IN; e2 = expr { Let (x, e1, e2) }
      | IF; e1 = expr; THEN; e2 = expr; ELSE; e3 = expr { If (e1, e2, e3) }
      | LPAREN; e=expr; RPAREN {e}
      ;
    %%
    ```


#### BNF {#bnf}

BNF stands for **Backus-Naur Form**. It's a way to describe the syntax of a
language it has the following form

```text
meta-variable ::= expression | ... | expression
```


#### Code Generation {#code-generation}

We could use the `dune` build system to compile the `.mll` and `.mly` files into
`.ml` files.
