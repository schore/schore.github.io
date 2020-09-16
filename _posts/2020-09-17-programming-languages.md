---
layout: post
title: Programming languages, which I learned
date: 2020-09-17
categories: software software-engeneering programming
author: Georg
---

In my past I got in contact with an number of programming languages. In this
post I will list them and extract the key points, I learned or liked from the
language. This can be a variety of different things, like how simple something is
or brilliant concepts.


## C/C++

I am at home in the embedded world. So C and it's successor C++ are the
languages the whole industry uses. Everything is possible with it, however it is
really easy to shoot yourself in the foot. The key benefit for this languages,
is to write device drivers. Also performance critical stuff may be written in C.

C/C++ was my first programming language, which I used as professional. So this
language was the door opener for my career in software engineering. What can you
learn from this language? Understanding of the hardware and device drivers. It
allows you to tinker with it. If you are interested in how the world is
controlled, this is an important language for you.

## Rust

The new kid on the block in the embedded world. I never tried it on bare metal
devices myself, however it should be possible. The essence of the language is to
enforce safe programming.

Rust doesn't require a runtime so it is possible to get it running on a bare
metal platform, which I never tried. My experience with this language is my toy
[project](https://github.com/schore/rustparser). So there are a number of
things, which you can get out of rust.

First of all, the concept of ownership and borrowing. This is brilliant, no race
conditions, no memory leaks and so on. It practically removes by design an
enormous amount of traps, which are well known in C.

Great type system. You get what you need. Struct and tuples are straight
forward. But the enums really rock.

```rust
enum IpAddr {
  V4(u8, u8, u8, u8),
  V6(String),
}
```

What do we see here, enums can also have a value. This is basically a kind of
variant datatype. But if you try to access it in an unsafe way, or even don't
consider defined values, you will get an compiler error. This makes your code
readable and robust at the same time.

What else, does this language have more to show. Yes it has! This was the first
modern language, I encountered, which has no classes. My first thought wtf, why?
But this time is long gone. The object oriented paradigm is on the way down and
Rust shows how we can get over it.

## Python

Personally I am not a big fan of python but this language has benefits and is
definitely one to look at. It is just so simple to get started and you can learn
it really quick. It is also often used for build scripts or to automate routine
tasks, and there it is really great. I get in touch with this language for
automation and you will probably too. What's missing in my humbled opinion is a
type system. I like languages which allows, at least to define the function
arguments.

## Haskell

What a language. I only played with it and it is hard to get your head around.
It was my first encounter with functional programming. The mathematical terms
used in the documentation and library doesn't make the life easier. What did I
get out of the language.

First of all, immutability is not a problem at all. It is a solution! No side
effects in the language are a great feature. If you don't have side effects,
reasoning is easy. That enables you to read and test your code.

And what else, partial function application.

```haskell
plus:: Num a => a -> a -> a
plus a b = a + b

add3 :: Num a => a -> a
add3 = plus 3
```
You don't have to know each parameter of an function. Just apply some of the
parameter and you get a knew function. It is simple, however realizing that
this concept is powerful took me a long time.

Lambdas, I was familiar with this concept, however hasn't realized the power of
them. I am looking back now at the design patterns of the gang of four and see
them in a different light. These patterns solves how to inject functions from
outside. So basically the problems lambdas and higher order functions solve.

I recommend you to have a look at this language. It really boosted my
understanding of computer science and broke my fixed view on object orientation.

## Clojure

This is another really interesting language, which is based on lisp. It shows
you that almost no syntax is required in for an programming language.
This is so powerful because it allows meta programming. You write
programs, which basically write programs. Even "core" functionality of this
language, is written with itself, using macros.

So what you can get out of it. Simplicity matters! The language is simple and
fulfills the requirements of a modern programming language. It's core concept
is published in 1958 and still forms the base of a modern programming language.
So learning one lisp like language will serve you well.

## Xtend

The last language, which I used professionally is xtend. It's translated into
java and afterwards compiled to machine code. This language doesn't give you a
lot of new concepts and it's not worth learning this language for education. I
have used it to generate C/C++ code from a domain specific language using xtext.
So it's basically a technology choice, which was made for this project and it
served us well. But don't learn it to broaden your horizon.


## Conclusion


What is the next programming language you should have a look at. The answer is as
always it depends.

| Language | reason                                                         |
|----------|----------------------------------------------------------------|
| Rust     | You want to be on a bare metal platform and fiddle with bits   |
| Haskell  | Getting into functional programming. These language is for you |
| Clojure  | Try something different and see the new old world              |

However, there are a lot of languages out in the wild. Choose the language,
which is fitting your task and career path. There is also another advice I came
across, while reading "The pragmatic programmer". Learn the language of your
editor. This is the program you will spend most of the time, so learn to control
it.
