# Introduction

This book aims to explain systems programming using Rust. It requires knowledge
of rust syntax, which can be found in the [Rust
book](https://doc.rust-lang.org/book/). This book is concerned with the
problems Rust aims to solve.

*Chapter 1* looks at a physical computer, and the instructions that a
computer will understand. We will see that this format is difficult to work in,
so we will see a new language (*C*) that allows us to think in a more natural
way, but can unambiguously be translated into machine instructions.

*Chapter 2* will look at some of the extra abstractions Rust makes on
top of the ones offered by C, and how this helps the compiler to spot when the
programmer makes a mistake. 

Subsequent chapters will look at how Rust can be used to solve example problems
in different areas.

*Chapter 3* will look at using rust for general automation jobs, in a way that
languages like *sh* have been used in the past. We will see that while Rust is
more verbose, it can be easier to reason about how the script will run.

In *chapter 4*, we will see how we can use Rust's type system to reduce errors
during data serialization, including to IO streams like TCP sockets, and to
storage systems like databases.

*Chapter 5* sees us create an operating system, again using Rust's type system
to provide a more reliable kernel where we know certain errors will be caught
by the type system. We will see that not all errors can be caught this way,
however, and that when working with low-level code that by its nature cannot
be checked, we must tread very carefully.

*Chapter 6* focuses on visual output devices and user interfaces. We will look
at how rust can help us write code for existing graphics APIs, and how some
Rust developers are exploring if it is possible to use Rust's type system to
catch incorrect usage of these APIs at compile time.

In *chapter 7* we look at interfacing with a foreign (C) library, where the 
difficulties lie in doing this, and how wrapping an API like this can expose
ambiguities in ownership in the C library, where lifetimes do not exist.
