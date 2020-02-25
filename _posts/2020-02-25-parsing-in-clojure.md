---
layout: post
title: Writing an parser in clojure
date: 2020-02-25
categories: programming clojure
author: Gorg
---

I am not sure why, but I like to write a parser. There are my learning projects
for a new programming languages. In order to achieve this goal it is necessary
to ensure, that you understand, what a parser is and how you can combine them.

## What is a parser

A parser is function which takes a string and returns maybe a result of any type
and the rest of the string. The type signature in haskell my look something like
this.

```haskell
type Parser a = String -> Maybe (a, String)
```

So a Parser is a function, which takes an String and returns a type and the rest
of the string. It is very important, that ```a``` is a variable type. 
This signature allows to implement parsers with different kind of complexities.
One which only returns a static value on the one hand and on the other hand a parser
for an complete programming language like C++.

So the challenge now is to combine parser, so you can build up your syntax with
small simple parser.

## Transform the parser

If you have an parser which gives you an string of numbers and it is necessary
to return an integer, transforming the parser is necessary. In haskell you would
write a function like fmap.

```haskell
fmap :: ( a -> b) -> Parser a -> Parser b
```

In the next snippet you can see the example to transform an character array of
numbers to an integer. In this snippet it is not obvious, that
```pUnsignedInt``` is an function, which takes an string and returns an Integer.

```clojure
(def pUnsignedInt (fmap #(read-string (apply str %))
                        (psome pNumber)))
```

It is even possible to construct an parser which evaluates the given string.


## Combining parser

```
transition stateA ->         stateB
<keyword>  <id>   <keyword>  <id>
```
The next challenge is to combine parsers, so that it's possible to get the ```<id>``` 
values from and to. The type signature in haskell would look exactly
like this. In haskell you would use the following type signature for the combination.

```haskell
>>= :: Parser a -> ( a -> Parser b ) -> Parser b
```

The trick here is, to use the ```>>=``` function also as part of the function ``` ( a -> Parser b )``` 
So here is an example from clojure.

```clojure
(>>= cleanWhitespace (fn ([_] 
(>>= (pkeyword "transition") (fn ([_] 
(>>= cleanWhitespace (fn ([_] 
(>>= pword (fn ([from] 
(>>= cleanWhitespace (fn ([_] 
(>>= (pkeyword "->") (fn ([_] 
(>>= cleanWhitespace (fn ([_] 
(>>= pword (fn ([to] 
    (pure {:transiton true, :from from, :to to}))))))))))))))))))))))))
```

So the first parser gets applied. If it is successful the function given by the
second argument is applied. This function also executes ```>>=```. So the 
parser ```(pkeyword "transition")``` gets applied. This goes on until the parser
created by ``` (pure {:transiton true, :from from, :to to})``` gets returned.
The values ```from``` and ```to``` are extracted from an arbitrary function
parameter.

## Conclusion

In writing this parser I think I got functors, applicative and monads. However I
am not 100% sure. This is hard topic. If you are interested in the sources check
it out on [github](https://github.com/schore/parser).

There are also some additional readings which are really useful and in a higher
quality than this blog post.

* <http://learnyouahaskell.com/functors-applicative-functors-and-monoids>
* <http://learnyouahaskell.com/a-fistful-of-monads>
* <http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html>
* <http://leonardoborges.com/writings/2012/11/30/monads-in-small-bites-part-i-functors/>
