---
layout: post
title: Logic programming
date: 2020-10-4
categories: software software-engeneering programming
author: Georg
---

at the moment I am getting in touch with a branch of programming known as logic
programming. This allows to search for solutions for a problem which is
described in the program. I had a look at prolog and clojure, which both served
me well.

This code is one of the standard examples. It's just the computation of the
faculty of a number x and the result is r.

```clojure
(defn fac [x r]
  (l/conde
   [(l/== x 1) ;;if x = 1 then r = 1
    (l/== r 1)]
   [(l/fresh [a decx] ;;fac x = x * fac (x-1)
      (fd/* a x r)
      (fd/- x 1 decx)
      (fac decx a))]))
```

So let's solve the faculty of 5
```clojure
(l/run 1 [q] (fac 5 q)) ;; => (120)
```

It is simpler with normal code, or? I agree here. But now let the magic happen.
Run the program backwards.

```clojure
(l/run 1 [q] (fac q 120));; => (5)
```

This is incredible, the same code for both operation. Of course this is not as
efficient as dedicated functions, but who cares. So what it is good for?

## What is it good for?

Now my learning project, the parser comes back again. I implemented it, again.
The code was straight forward and looks really simple, but that's not the point.
I also assumed that the input is already tokenized. But now let the magic happen.
Look at the next snippet!

```clojure
(defn completion [parser inp suggestion]
  (l/fresh [complete completed _dc]
    (l/appendo inp complete completed)
    (parser completed '() _dc)
    (l/firsto complete suggestion)))
```

That is the complete code, which allows to get valid suggestions from an
partially written program. Code completion in only five lines of code. I
expected it to be much harder and this is really amazing.

## Conclusion

Should you have a look on logic programming. It is another tool in your toolbox,
which allows to solve an additional type of problems. So take a look at it.
Prolog or clojure.core.logic is a good starting point.
