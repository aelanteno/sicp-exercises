---
layout: default
---

> [Exercise 1.7](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.7). The `good-enough?` test used in computing square roots will not be very effective for finding the square roots of very small numbers. Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers. Explain these statements, with examples showing how the test fails for small and large numbers. An alternative strategy for implementing `good-enough?` is to watch how `guess` changes from one iteration to the next and to stop when the change is a very small fraction of the guess. Design a square-root procedure that uses this kind of end test. Does this work better for small and large numbers?

For reference, here is the set of procedures used in this section of the text for calculating square roots.

```scheme
(define (sqrt x)
  (sqrt-iter 1.0 x))

(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x)
                 x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001))

(define (square x)
  (* x x))
```

## Why is this not very effective for very small numbers?

The amount of error that is good enough in the definition of `good-enough?` is 0.001. For very small numbers, that's proportionally a lot of error.

Here's an example with the very small number 0.000000001. Take the square root of it using these procedures, then square that result:

```
> (sqrt 0.000000001)
0.03125001065624928
> (square 0.03125001065624928)
0.0009765631660156936
```

We end up with a number that's several orders of magnitude off from the original number.

## Why is this not very effective for very large numbers?

With very large numbers, there is often an infinite loop where, after a while, `(improve guess)` is the same as `guess`, but `good-enough?` returns `#f`.

Example: 40 9's in a row works in my setup. 50 9's in a row gets stuck.

```scheme
> (sqrt 99999999)
9999.99995
> (sqrt 99999999999999999999)
10000000000.0
> (sqrt 9999999999999999999999999999999999999999)
1e+20
> (sqrt 99999999999999999999999999999999999999999999999999)
. . user break
```
However, I don't understand why this infinite loop happens so often. The error in how a computer represents very large numbers gets to be large in absolute terms when working with very large numbers. So `guess` and `(improve guess)` could be represented by the same number even though they're actually far from each other in absolute terms. But I don't know why that wouldn't usually result in `(square guess)` and `x` also being represented by the same number and the program therefore terminating.

## A new `sqrt` procedure that stops when the change from one guess to the next is a very small fraction of the guess

I added an argument to `sqrt-iter` and `good-enough?` to represent the previous guess. I removed `x` as an argument to `good-enough?` because now we care about the relationship between the new guess and the previous guess, not the relationship between the new guess and `x`. I added "-new" to the names of the procedures that I changed.

```scheme
(define (sqrt-new x)
  (if (= x 0)
      0
      (sqrt-iter-new 0.0 1.0 x)))

(define (sqrt-iter-new previous-guess guess x)
  (if (good-enough-new? previous-guess guess)
      guess
      (sqrt-iter-new guess (improve guess x) x)))

(define (good-enough-new? previous-guess guess)
  (< (abs (/ (- previous-guess guess) guess)) 0.001))
```

**Note 1:** There's a check in `sqrt-new` to see if x = 0. Without that, there is an infinite loop when x = 0. Each guess is 1/2 of the previous guess, so the difference between the previous guess and the current guess is always the same as the current guess. The proportion between the two is always 1 and can never get below 0.001.

**Note 2:** On [this page](http://community.schemewiki.org/?sicp-ex-1.7), GWS says you can have `good-enough?` stop when the new guess *equals* the previous guess. Because computers have limited precision, the program will always reach a point where that happens. You will get the most accurate answer that your computer can give. I don't know enough to know whether that approach will always work, so I stuck with my answer. But it's a cool idea.

## Do the new versions work better for very small numbers?

For very small numbers, `sqrt-iter-new` runs more times than `sqrt-iter` does. This is because the change from guess to guess has to be very small in order to have the proportion between the change and the guess to be less than 0.001, since the guess itself is so small. So for very small numbers, `sqrt-new` returns a more accurate answer than `sqrt` does.

Here's the same example as above. When we take `sqrt-new` of 0.000000001 and then square the result, the answer is much closer to 0.000000001 than it was above.

```scheme
> (sqrt-new 0.000000001)
3.162278058889937e-05
> (square 3.162278058889937e-05)
1.0000002521736707e-09
```

## Do the new versions work better for very large numbers?

For very large numbers, `sqrt-iter-new` runs *fewer* times than `sqrt-iter` does. This is because the change from guess to guess doesn't have to be very small in order to have the proportion between the change and the guess to be less than 0.001, since the guess itself is so large.

The original version gets stuck on 50 9's in a row, but the new version doesn't. It doesn't even get stuck on 60 or 100 9's in a row. And the answers are very close to the exact square roots.

```scheme
> (sqrt-new 99999999999999999999999999999999999999999999999999)
1.0000003807575104e+25
> (sqrt-new 999999999999999999999999999999999999999999999999999999999999)
1.0000000031080746e+30
> (sqrt-new 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999)
1.0000000000002003e+50
```

## Smaller error

I left the error amount at 0.001 to make my procedure more comparable to the original. But then I tried it with an error of 0.000000001 instead. As expected, that gave more accurate results:

```scheme
> (sqrt-new 0.000000001)
3.1622776601683795e-05
> (square 3.1622776601683795e-05)
1e-09
```

```scheme
> (sqrt-new 99999999999999999999999999999999999999999999999999)
1e+25
> (sqrt-new 999999999999999999999999999999999999999999999999999999999999)
1e+30
> (sqrt-new 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999)
1e+50
```
