+++
title = "OCaml Learning-II"
author = ["Yusheng Zhao"]
date = 2023-12-30T00:00:00+08:00
tags = ["OCaml", "PackageManagement"]
draft = false
+++

## Why {#why}

This brief note serves to record project management tools and precedures for
creating `OCaml` projects.


## How {#how}


### Package Management {#package-management}

The most important aspect of open-source development is for your code to be
published and utilized by other users. In order to do that, it is essential to
have a **replicable** environment and **clean** project structure. The
`PkgTemplates.jl` is a `Julia` package for creating new `Julia` packages. It has
spoiled me for being **easy** to use, having **clean** project generation, having
**no extra dependency** on other tools, and has **good synergy** with github. In
`OCaml` ecosystem, there are few options like `PkgTemplates.jl` such as `spin`
and `pesy`. However, `drom`, a wrapper over opam and dune, resembles
`PkgTemplates.jl` the most. Therefore, I will adopt to use it. It should be
noted however the other two tools has their own strengths. For example, `spin`
has more templates than `drom`. Underthe hood, `drom` uses `opam` for
environment management and `dune` for project management and project building.


#### Drom Installation {#drom-installation}

`drom` is a `OCaml` package. Therefore, it can be installed using `opam`. To do
so, use the following command:

```bash
opam install drom
```


#### Drom Usage {#drom-usage}

In order to create a project skeleton, we could use the following code

```sh
drom new PKG-NAME --skeleton SKELETON-NAME
```

There are six possible `SKELETON-NAME`:

1.  `virtual`
2.  `library`
3.  `program`
4.  `mini-lib`
5.  `mini-prg`
6.  `ppx_deriver`

The details of each skeleton is beyond the scope of this note. Interested reader
will find their [documentation](https://ocamlpro.github.io/drom/sphinx/reference.html#skeletons) helpful. We will continue this note with `program`
skeleton. Within the `program` project structure, there are two sub-structure in
the `src` folder. They are known as the **driver**, it has the same name as the
library, and the **library**, it has the name appended with the `_lib` postfix.
The driver contains all executable functions of the project. It may call
functions in the **library** or outer libraries. The **library** serves as a place
for providing helper or general features that can potentially be used in other
libraries and the current project but is not implement anywhere else. As of
writing this note, it is not clear how to add other `library` to an existing
package. A possible way will be to manually add `[package]` section to the main
project `drom.toml` file.

<!--list-separator-->

-  Customization

    There are a few things we would like to change based on the skeleton. We could
    do that by first editing the `drom.toml` file and them run `drom project` to
    update the project structure. More specifically, we could also change the
    configuration for the package content or the package library content.

    <!--list-separator-->

    -  Menhir

        For more details, please refer to [drom documentation](https://ocamlpro.github.io/drom/sphinx/usecases.html).

<!--list-separator-->

-  Build Project

    You may build the project with `drom build -y` and then run it with `drom run`.
    There are other convinent options like building the documentation and installing
    the written package into a global `opam` environment. You may find details in
    [drom's documentation](https://ocamlpro.github.io/drom/sphinx/quickstart.html#installing-the-project) helpful.


## How to manage environment in OCaml {#how-to-manage-environment-in-ocaml}

No language is of any use without good libraries. The libraries in OCaml are
managed by a tool called `opam`. The environments in OCamel are termed as
switches. To create a new switch, use the following command:

```bash
opam switch create <switch-name> <compiler-version>
```

For example, to create a switch named `ocaml-4.10.0`, use the following
command:

```bash
opam switch create ocaml-4.10.0 ocaml-base-compiler.4.10.0
```

To list all the switches, use the following command:

```bash
opam switch list
```

To switch to a different switch, use the following command:

```bash
opam switch set <switch-name>
```


### Resources {#resources}

-   [OCaml Official Guide](https://ocaml.org/docs/opam-switch-introduction)


## How to create a Project in OCaml {#how-to-create-a-project-in-ocaml}

The tool that is needed to create a project in OCaml is called `dune`. To create
a new project, you could do

```nil
dune init proj hello
```


### Modules in OCaml {#modules-in-ocaml}

Modules in OCaml are similar to namespaces in C++. They are used to organize
functions and types. Each module is defined in a file with the same name as the
module. For example, the module `List` is defined in the file `List.ml`. The

`lib/ModuleName.ml` is the file that defines the module `Module`. The
`lib/ModuleName.mli` is the file where you decide which functions and types to
make public.


### Install Modules {#install-modules}

`opam isntall packageName`


### Resources {#resources}

-   [OCaml Official Guide](https://ocaml.org/docs/your-first-program)


## Resources {#resources}

-   [OCamlverse](https://ocamlverse.net/content/build_systems.html)
