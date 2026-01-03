# Forward

I mentioned using Nixos in my [Home Lab post](https://github.com/jusanherndon/blog/blob/master/tech/home%20lab/home%20lab.md#lastly-i-have-my-nixos-pc), and here is where I am going to go into more detail.

# Introduction

The Nix ecosystem is kinda complex and hard to understand at first. I am going to try and break down Nix like what I saw in this [video](https://www.youtube.com/watch?v=5D3nUU1OVx8) from Surma. Their are four aspects I want to dive into for this discussion. What is the motivation for the Nix Ecosystem? What is Nix? What are Nix packages? and why have a seperate Linux distrobution for all of this?

# Motivation for the Nix project

I first want to start at the beginning. Here is the link to the [Nix PHD Thesis](https://edolstra.github.io/pubs/phd-thesis.pdf). You do not have to read the whole thing, but I think its worth looking deeper into if you want to better understand the computer problems involved and what Nix tries to do to fix them.

So lets get started. What is the reason Nix exists? Page 11 of the thesis says this: "This thesis is about getting computer programs from one machine to another—and having them still work when they get there. This is the problem of *software deployment*." 

Nix was created to help deploy software on multiple machines. Their are several issues in software deployment, but Nix mainly focuses on two different kinds. Environment and Manageability issues.

## Environment issues

 

## Manageability issues


# Aside for how software gets built 

I feel like this section is necessary because there are several different ways software can be run and deployed. The PHD Thesis mainly focuses on the **C programming language**. Where software gets built using compilation and a Linking phase. Nix handles many different progamming languages like Python which is an interpreted language. Python has a interpreter written in C and needs to be installed on every computer running Python. For now I am going to use C in this explanation and you can look up other software langauages in your own time.

## Compilation


## Linking

# Nix Package Manager and Nix programming Language



# Nix Packages



# Nixos


[Nix Garbage collect](https://nix.dev/manual/nix/2.28/command-ref/nix-collect-garbage.html)
[Nix generations](https://blog.bitclvx.com/posts/nix-generations/)

Other Learning resources:
[No Boilerplate](https://www.youtube.com/watch?v=CwfKlX3rA6E)
