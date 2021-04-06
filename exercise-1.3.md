---
layout: default
---

> [Exercise 1.3](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.3). Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.

I used the definitions of `square` and `sum-of-squares` that are in the text:

``` scheme
(define (square x) (* x x))

(define (sum-of-squares x y)
  (+ (square x) (square y)))
```

### My First Solution

This procedure to find the sum of the squares of the two larger numbers in a triple uses `cond`, which was introduced in this section of the book. The first line of the `cond` checks if `a`  is both <= `b` and <= `c`. If so, `b` and `c` will be the two larger numbers, so we call `sum-of-squares` with them. If that first line isn't true, then we know that `a` is one of the two larger numbers so we can just check if `b` <= `c`. If it is, then we can pick `a` and `c` for the two larger numbers to use in `sum-of-squares`. If not, then the last case is that `a` and `b` are the two larger numbers, and we use them in `sum-of-squares`. Note that if two or three of the input numbers are the same, it doesn't matter which we pick since the sum of the squares will be the same.

``` scheme
(define (sum-of-squares-of-two-larger-numbers a b c)
  (cond ((and (<= a b) (<= a c)) (sum-of-squares b c))
        ((<= b c) (sum-of-squares a c))
        (else (sum-of-squares a b))))
```

### A More Readable Version

Someone pointed out to me that my first attempt isn't very readable. That's why I included a paragraph of explanation before it. It would be better to have the program clear enough that explanation isn't necessary.

For my next version, I thought about what's happening in the procedure. Two things:
- finding the larger two of the three given numbers
- finding the sum of the squares of those two numbers

So I wrote procedures to do those things separately, splitting up the first thing further into finding the largest number and finding the second largest number.

``` scheme
(define (sum-of-squares-of-two-largest-numbers x y z)
  (sum-of-squares
     (largest-of-three-numbers x y z)
     (second-largest-of-three-numbers x y z)))
     
(define (largest-of-three-numbers x y z)
  (max (max x y) (max y z)))

(define (second-largest-of-three-numbers x y z)
  (if (< x z)
      (min (max x y) z)
      (max (min x y) z)))

(define (min x y) (if (< x y) x y))

(define (max x y) (if (> x y) x y))
```

