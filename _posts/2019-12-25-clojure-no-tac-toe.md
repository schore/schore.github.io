---
layout: post
title: Programming NoTacToe in clojure(script)
date: 2019-12-25 
categories: programming clojure
author: Gorg
excerpt_separator: <!--more-->
---

In order to get a first overview in clojure I decided to program tic tac toe in
clojure. However there is also a clojure to javascript compiler, which allows me
to use a web browser for visualization. So I used clojurescript.

![notactoe]({{site.baseurl}}/asset/notactoe.png)

<!--more-->

In this picture all winning moves are green, loosing moves are red.

## The game

The game is a variant of tic tac toe only with crosses. I found the rules of the
game, by watching [NumberPhile on
Youtube](https://www.youtube.com/watch?v=ktPvjr1tiKk). This game is solved,
which means that the first player can always win on a three board game. The
winning strategy is explained in the
[paper](https://arxiv.org/pdf/1301.1672v1.pdf).

## The implementation

You can find the whole code of the implementation on
[github](https://github.com/schore/notactoe). The structure of the code is not
consistent and far away from being perfect. It is my first clojure project.

### Getting the value of a single board

The first task was to map the different boards to it's misere quotient.

```clojure
(def nonisomorphic-map
  {[[0 0 0] [0 0 0] [0 0 0]] [:c]
   [[1 0 0] [0 0 0] [0 0 0]] []
   [[0 1 0] [0 0 0] [0 0 0]] []
   [[0 0 0] [0 1 0] [0 0 0]] [:c :c]
   [[1 1 0] [0 0 0] [0 0 0]] [:a :d]
   ... })
```

However there are many position which are equivalent and so the computer should
do this work. Therefor I created two function to rotate the board. So the
complete map is created. The last step is pretty easy. Get the value by a lookup
in the map.

```clojure
(defn mirror-map
  [input]
  (let [k (first input)
        v (second input)]
    [[k v]
     [(mirror-board-horizontal k) v]
     [(mirror-board-vertical k) v]
     [(mirror-board-vertical (mirror-board-horizontal k)) v]]))


(defn rotate-map
  [input]
  (let [k (first input)
        v (second input)]
    (for [i (range 4)]
      [(rotate-board k i) v])))

(defn complete-map-generator
  [input]
  (->> input
       (mapcat mirror-map)
       (mapcat rotate-map)
       (into {})))
```

### Calculating the misere quotient

After the lookup in the table of more then one board i get an structure like [:a
:b :a :c]. Because this is hard to work with a conversion to the simpler form
{:a 2 :b 0 :c 1 :d 0} is required. This allows me to use the following function
to compute the board. This table is also from the paper. The second function
applies the reduce-table function until nothing changes.

```clojure
(defn reduce-table
  "Reduces a complex input to a simpler form"
  [input]
  (cond
    (>= (:a input) 2) (update input :a #(- % 2))
    (>= (:b input) 3) (update input :b #(- % 2))
    (and (>= (:b input) 2)
         (>= (:c input) 1)) (update input :b #(- % 2))
    (>= (:c input) 3) (-> input
                          (update :a inc)
                          (update :c dec))
    (and (>= (:b input) 2)
         (>= (:d input) 1)) (update input :b #(- % 2))
    (and (>= (:c input) 1)
         (>= (:d input) 1)) (-> input
                                (update :a inc)
                                (update :c dec))
    (>= (:d input) 2) (-> input
                          (update :c (partial + 2))
                          (update :d #(- % 2)))
    :else input))

(defn reduce-board
  "reduces teh game to it's simplest form"
  [board]
  (let [newboard (reduce-table board)]
    (if (= newboard board)
      board
      (recur newboard))))
```

Everything else in this project are functions to display or select the moves for
visualization and testing. However this function are far from perfect or
complete.


## Testing

There is also an important topic which I scratched only on the edge testing. I
had no idea if my code is working. Especially the reduce functions. I used
[test.check](https://clojure.github.io/test.check/intro.html) to verify my code.
The main idea is to define a property, which has to be fulfilled. A random input
is generated and the properties are checked against an input.

```clojure
(def gen-boards (gen/hash-map
                 :a gen/nat
                 :b gen/nat
                 :c gen/nat
                 :d gen/nat))

(def reduce-board-test
  (prop/for-all [a gen-boards]
                (some #(= (reduce-board a) %) (map tranfsform-table possible-endresults))))

(tc/quick-check 1000 reduce-board-test)
```

The 1000 passing random tests here made me pretty confident, to say "This
function is working" So I can go on with the project.

# Summary

Clojure has interesting concept and it is very easy to get started. However you
should understand the basic principles of functional programming. I have one
headache in clojure, the missing types. Maybe I can find the right way to handle
types in the spec library.

# Installation

So if you are interested, there are some steps in order to play the game.

- Install [lein](https://leiningen.org/)
- git clone https://github.com/schore/notactoe.git
- lein figwheel

I hope this works, however I am not sure. Don't hesitate to ask me in case of
problems.
   
