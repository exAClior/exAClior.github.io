---
title: "Juliabeginner"
date: 2023-06-07T21:43:39+08:00
draft: true
---
# Purpose
In this blog, I have collected some tutorials that I found useful while learning Julia. I will also provide a tool-chain that I like to use while coding in Julia. I hope this will speed up your process of learning Julia.

# Julia Installation
Sometimes it's necessary to have multiple versions of Julia on your machine. For example, you might need to test a package whose dependency relies on an earlier version of Julia.

A good tool for managing your Julia versions is [Juliaup](https://github.com/JuliaLang/juliaup).

# Package Development
As I was taught and proven by my own experience, the best way to learn Julia is by writing something. To organize the code you write, you should put it into a package. During the process, I found using [PkgTemplates.jl](https://github.com/JuliaCI/PkgTemplates.jl) helpful in terms of creating a structured repository. I also found using [Revise.jl](https://github.com/timholy/Revise.jl) helpful when you are in need of constant testing and code revisions.

A concise introduction to these two packages is provided on the [Coding Club page](https://github.com/CodingThrust/CodingClub/tree/main/julia-packages), thanks to [Prof. Jin-Guo Liu](https://giggleliu.github.io/).

# Editor
A good editor can make your development process much more enjoyable. Personally, I prefer using [Doom Emacs](https://github.com/doomemacs/doomemacs) with [Julia-Snail](https://github.com/gcv/julia-snail). However, Emacs comes with a very steep learning curve. Therefore, I would recommend beginners to use [VSCode](https://code.visualstudio.com/). However, don't forget to map your keys to Vim's. It helps.

# Tutorials 
- [Introduction to Julia | Jose Storopoli | JuliaCon
  2022](https://www.youtube.com/watch?v=uiQpwMQZBTA)
- [HKUST(GZ) First Coding Club Julia
  Tutorial](https://github.com/CodingThrust/CodingClub/tree/main/julia)
- 