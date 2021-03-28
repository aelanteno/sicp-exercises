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

My procedure to find the sum of the squares of the two larger numbers in a triple uses `cond`, which was introduced in this section. The first line of the `cond` checks if *a*  is both <= *b* and <= *c*. If so, *b* and *c* will be the two larger numbers, so we call `sum-of-squares` with them. If that first line isn't true, then we know that *a* is one of the two larger numbers so we can just check if *b* <= *c*. If it is, then we can pick *a* and *c* for the two larger numbers to use in `sum-of-squares`. If not, then the last case is that *a* and *b* are the two larger numbers, and we use them in `sum-of-squares`. Note that if two or three of the input numbers are the same, it doesn't matter which we pick since the sum of the squares will be the same.

``` scheme
(define (sum-larger-squares a b c)
  (cond ((and (<= a b) (<= a c)) (sum-of-squares b c))
        ((<= b c) (sum-of-squares a c))
        (else (sum-of-squares a b))))
```

I tested my procedure to see if it worked. It did.

    > (sum-larger-squares 1 2 3)
    13
    > (sum-larger-squares 1 3 2)
    13
    > (sum-larger-squares 3 2 1)
    13
    > (sum-larger-squares 3 1 2)
    13
    > (sum-larger-squares 2 1 3)
    13
    > (sum-larger-squares 2 3 1)
    13
