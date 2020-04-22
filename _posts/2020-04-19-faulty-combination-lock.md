---
author: Drew Cummins
usemathjax: true
---

This was a deceptively difficult and fun puzzle from the National Museum of Mathematics' [Mind-Benders for the Quarantined!](https://momath.org/civicrm/?page=CiviCRM&q=civicrm/event/info&reset=1&id=1620), with a lot of emotional dead ends and googling "permutation v combination"s and "wow, you really don't know anything about combinatorics, do you?"s.

Here's the problem statement, in all its cunning simplicity:

> A combination lock with three dials, each numbered 1 through 8, is defective in that you only need to get two of the numbers right to open the lock.  (For example, suppose the true combination is 4-2-7.  Then 4-2-7 would open the lock, but so would 4-2-5, 4-2-2, 8-2-7, or 4-6-7.  But not 2-4-7.)

> What is the minimum number of (three-number) combinations you need to try in order to be sure of opening the lock? 
$$\newcommand{\clr}[2]{\color{#1}{\small #2}}$$
$$\newcommand{\combo}[4]{\clr{#1}{#2} \text{-} \clr{#1}{#3} \text{-} \clr{#1}{#4}}$$

### A Quick Glossary

It's going to be a lot clearer if we explicitly define some terminology, primarily for situational disambiguation, but also because combination means something specific in mathematics that is unfortunately at odds with how it's used w.r.t. locks.

|:---|:---|
| **permutation** | a valid setting of the lock's dials, e.g. 3-8-1 |
| **configuration** | the permutation the lock-maker set it to |
| **guess** | a permutation we're hoping unlocks the lock |
| **cover** | a configuration is _covered_ if a guess would unlock it |

### Done and Dumb and Confident

At first glance, this seems simple: if one of the dials doesn't matter, we just have to try all possible permutations of 2 dials for a total of $$8^2 = 64$$ guesses (2 dials, 8 numbers each), after which we can be certain to have unlocked the damned thing. 

But this isn't actually the best we can do. Let's first consider the much smaller case of 3 dials numbered 1 through 2 instead, which at $$2^3=8$$ _total_ permutations is much easier to investigate. According to that initial premise, we would expect to find $$2^2=4$$ guesses necessary to guarantee solving such a lock, but it turns out we can do it in just _two_. 

It's easy to see how this is possible if we try the guesses $$\combo{Goldenrod}{1}{1}{1}$$ and $$\combo{DarkOrchid}{2}{2}{2}$$:

|:---:|:---:|
| $$\combo{Goldenrod}{1}{1}{1}$$ | $$\combo{DarkOrchid}{2}{2}{2}$$ |
| $$\combo{Goldenrod}{1}{1}{2}$$ | $$\combo{DarkOrchid}{2}{2}{1}$$ |
| $$\combo{Goldenrod}{1}{2}{1}$$ | $$\combo{DarkOrchid}{2}{1}{2}$$ |
| $$\combo{Goldenrod}{2}{1}{1}$$ | $$\combo{DarkOrchid}{1}{2}{2}$$ |

These are the only possible permutations of such a lock, and hopefully it's easy to see via the coloring that together, $$\combo{Goldenrod}{1}{1}{1}$$ and $$\combo{DarkOrchid}{2}{2}{2}$$ cover every possible configuration.

### Combinatoric Approach

Let's try to reason a bit about this by considering how many configurations are covered by a guess. There are 3 _pairs_ of dials: (1,2), (1,3) and (2,3). So a guess will cover all configurations that share _any_ of those pairs, since we only need 2 dials to match. So for each pair, we cover as many permutations as there are settings of the remaining dial—in this toy case 2, in the actual puzzle, 8. So that gives us 3 pairs times 2 settings of the remaining dial, minus the 2 extra times we counted our guess permutation: $$3\times2-2 = 4$$, and $$3\times8-2=22$$ for the actual puzzle.

This means each guess covers $$3n-2$$ configurations, where $$n$$ is how many numbers there are per dial. Since we've already shown 2 guesses in our toy case that cover all configurations (read: it's possible to do it in 2), and we know each guess covers 4 configurations and 4 is less than the 8 total permutations, this proves 2 would be the answer if it were just the toy puzzle we were solving.

Unfortunately, things don't work out so well for the actual puzzle—22 doesn't divide the $$8^3=512$$ total permutations. From this we can deduce that some of our guesses _must_ cover the same configurations, and so we know from this point out that the solution, at least from a counting-down combinatoric approach, must be a far hairier affair. We can, however, give a lower bound to the answer: $$24 = \left\lceil \frac{512}{22} \right\rceil$$.

I toiled with the way guesses overlapped, but didn't ever make any meaningful progress. You can't brute force it, because even with our lower bound, there are 98,173,718,189,790,929,581,466,857,579,452,509,896,000 unique subsets of 24 guesses. That number is probably LaRgEr ThAn tHe NuMbEr oF aToMs iN ThE uNiVErSe or something, but I think everyone's tired of hearing that sort of thing.

### Geometric Approach

If we instead think of each permutation as a point in space (the first dial moves us along the x-axis, the second dial along the y-axis and the third dial along the z-axis), we end up with an $$n\times n\times n$$ cube with some neat properties.

Each guess is now a point, and each covered configuration is _collinear_ with that guess, and parallel to the x, y or z axes. Another way of thinking about it is that a guess sits at the intersection of 3 lines—each of those lines represents a broken dial: if we have dial 1 (x-axis) and 2 (y-axis) correct, then all configurations with those (x,y) coordinates are covered, regardless of dial 3 (z-axis). This gives us some spatial intuition for how to construct our list of guesses.

This is probably easier to just look at:

![Unlocking the toy lock](/assets/images/faulty-lock/toy-lock-geometry.png)

First we've plotted all possible permutations of our toy lock, then in the middle diagram, we guess $$\combo{Goldenrod}{1}{1}{1}$$ and color each configuration this guess covers, and then finally guess $$\combo{DarkOrchid}{2}{2}{2}$$, at which point it should be apparent we've covered all possible configurations.

It might not have been apparent before, but now we can easily find other pairs of guesses that also unlock our toy lock, e.g. $$\combo{Goldenrod}{1}{1}{2}$$ and $$\combo{DarkOrchid}{2}{2}{1}$$:

![Another solution](/assets/images/faulty-lock/toy-lock-121.png)

Unfortunately even this approach becomes on its own quickly unwieldy for larger $$n$$. Look at our first guess and a completed version for $$n=4$$:

![Another solution](/assets/images/faulty-lock/cube-4-all.png)

It's just not immediately apparent (or wasn't, to me) how we minimize overlap in our guesses. If we take a slightly different tack and constrain ourselves to a single (x,y) plane in space at a time, a few things jump out:

![Another solution](/assets/images/faulty-lock/cube-4-xy-11-wide.png)

First, this initial guess will cover $$2n-1$$ configurations, $$7$$ for this case of $$n=4$$. If this was the only guess we had on this plane, what is the _minimum_ number of guesses we'd have to make on other planes to cover each remaining configuration on this plane? $$9$$, because a guess on a different plane can only cover a single configuration on _this_ plane, and we have $$9$$ currently uncovered configurations.

![Another solution](/assets/images/faulty-lock/cube-4-xy-22-wide.png)

Next, our second guess covers at most $$2n-3$$ ($$5$$ in this case) new configurations. 