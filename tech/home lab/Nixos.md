# Forward

I mentioned using Nixos in my [Home Lab post](https://github.com/jusanherndon/blog/blob/master/tech/home%20lab/home%20lab.md#lastly-i-have-my-nixos-pc), and here is where I am going to go into more detail.

# Introduction

The Nix ecosystem is kinda complex and hard to understand at first. I am going to try and break down Nix like what I saw in this [video](https://www.youtube.com/watch?v=CwfKlX3rA6E) from Surma. Their are four aspects I want to dive into for this discussion. What is the motivation for the Nix Ecosystem? What is Nix? What are Nix packages? and why have a seperate Linux distrobution for all of this?

# Motivation for Nix project

I first want to start at the beginning. Here is the link to the [Nix PHD Thesis](https://edolstra.github.io/pubs/phd-thesis.pdf). You do not have to read the whole thing, but I think its worth looking deeper into if you want to better understand the computer problems involved and what Nix tries to do to fix them.

One thing I like to get out of the way is that Nix cannot fix bad software. Its a Packaging schema that tries to solve the issues that arise on Linux because of dynamic linking of binaries. 

So lets get started. What is the reason Nix exists. Page 11 of the thesis says this: "This thesis is about getting computer programs from one machine to another—and having them still work when they get there. This is the problem of *software deployment*." 

Nix was created to help deploy software on multiple machines. Their are several issues in software deployment, but Nix mainly focuses on two different kinds. Environment and Manageability issues.

# Nix Package Manager and Nix programming Language



# Nix Packages



# Nixos


[Nix Garbage collect](https://nix.dev/manual/nix/2.28/command-ref/nix-collect-garbage.html)
[Nix generations](https://blog.bitclvx.com/posts/nix-generations/)

Other Learning resources:
[No Boilerplate](https://www.youtube.com/watch?v=CwfKlX3rA6E)
