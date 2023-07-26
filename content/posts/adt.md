+++
title = "Algebraic Data Type"
author = "Yusheng Zhao"
date = 2023-07-04T00:00:00+08:00
tags = ["Programming"]
draft = false
+++

## Motivation {#motivation}

In my ongoing project on [ZX Calculus](https://github.com/QuantumBFS/ZXCalculus.jl), I found myself needing an elegant and
efficient way to handle objects of specific types and perform operations on
their values. Enter Algebraic Data Types (ADTs) - a powerful concept from
functional programming. ADTs, coupled with pattern matching, provide a solution
that is not only efficient but also expressive, allowing for cleaner, more
readable code when dealing with complex data structures.


## Algebraic Data Types {#algebraic-data-types}

Most programming languages provide types to represent primitive data. For
instance, digit numbers are typically represented in `Int`, and alphabets in
`char`. However, these data types alone might not always convey significant
meaning. It's through their composition that we can represent more complex
concepts. This is where Algebraic Data Types, first introduced in functional
programming, come into play. An Algebraic Data Type (ADT) is a type formulated
by composing other types. The algebra in its name encapsulates the operations
you may execute on these composed types. You can perform addition on types to
create a **Sum Type**. As an example, in Julia we can define:

```julia
@enum Shape Circle=1 Rectangle=2 Square=3 RightTriangle=4
```

This is a sum type because the total number of possible instances of the ADT
`Shape` is the sum of its `variants`, i.e., `Circle`, `Rectangle`, etc. You can
also perform multiplication on types to form a **Product Type**.

```julia
struct Rectangle
length::Int32
width::Int32
end
```

This is a product type because the total number of potential instances of
`Rectangle` is the product of the possible values for `length` and `width`.
Consequently, you can further compose Sum Types and Product Types to create more
intricate Algebraic Data Types. Here is an example of defining a Product Type
using Expronicon.jl:

```julia
using Expronicon.ADT: @adt

@adt public Shape begin

    struct Circle
        radius::Float32
    end

    struct Rectangle
        length::Int32
        width::Int32
    end

    struct Square
        side::Int32
    end

    struct RightTriangle
        side1::Int32
        side2::Int32
    end
end
```

Note the usage of the keyword `public`; this implies we can use `Circle` instead
of `Shape.Circle`, making our code more readable and easier to manage.


## Pattern Matching {#pattern-matching}

While representing complex concepts through grouped data is beneficial,
manipulating this data is equally critical. The first step to handling data is
through extraction, which is achieved by **destructuring** an Algebraic Data Type.
This can be generalized into the concept of **pattern matching**, defined as 'the
act of examining a given sequence of tokens for the presence of the constituents
of some pattern' [[1]​](https://en.wikipedia.org/wiki/Pattern_matching). A comprehensive list of pattern matching rules can be
found in MlStyle.jl's documentation. For the sake of brevity and relevance, I'll
provide examples of pattern matching using the ADT defined with Expronicon and
the pattern matching feature provided by MLStyle.jl. This choice was made
because the ADT in MLStyle.jl supports generic types, so it won't be type
stable. For performance reasons, I'll use the ADT from Expronicon. Both packages
interact well.

```julia
function enlarge_shape(s::Shape)
    @match s begin
        Circle(r) => Circle(2r)
        Rectangle(l,w) => Rectangle(2l,3w)
        Square(s) => Square(s^2)
        RightTriangle(a,b) && if => RightTriangle(2a,2*b)
        end
end
```

This code accepts an instance of the Algebraic Data Type `Shape` and enlarges
the figure according to the specific type of each passed instance.


## Performance Analysis {#performance-analysis}

Roger, author of Expronicon.jl, has made a [comparison](https://expronicon.rogerluo.dev/intro/adts/intro) between using ADT and
naive `isa :` ternary operator. It has clear performance gain. For the sake of
time, I will not try to reproduce them here.


## Disclaimer {#disclaimer}

This post has been lovingly crafted using [Org Babel](https://orgmode.org/worg/org-contrib/babel/), a literal programming
support application in Emacs.


## References {#references}

-   [What's the point of having Algebraic Data Types?](https://qr.ae/pybRzN)
-   [Introduction to ADT](https://jnkr.tech/blog/introduction-to-algebraic-data-types)
-   [MLStyle.jl](https://github.com/thautwarm/MLStyle.jl)
-   [Expronicon.jl](https://github.com/Roger-luo/Expronicon.jl)
