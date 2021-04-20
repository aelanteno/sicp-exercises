---
layout: default
---

> [Exercise 1.10](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.10). The following procedure computes a mathematical function called Ackermann's function.
>
```scheme
> (define (A x y)
>   (cond ((= y 0) 0)
>         ((= x 0) (* 2 y))
>         ((= y 1) 2)
>         (else (A (- x 1)
>                  (A x (- y 1))))))
```
>
> What are the values of the following expressions?
>
```scheme
> (A 1 10)
> 
> (A 2 4)
> 
> (A 3 3)
```
>
> Consider the following procedures, where A is the procedure defined above:
>
```scheme
> (define (f n) (A 0 n))
> 
> (define (g n) (A 1 n))
> 
> (define (h n) (A 2 n))
> 
> (define (k n) (* 5 n n))
```
>
> Give concise mathematical definitions for the functions computed by the procedures f, g, and h for positive integer values of n. For example, (k n) computes 5n2.

### (A 1 10)

```scheme
(A 1 10)
= (A 0 (A 1 9))
= (A 0 (A 0 (A 1 8)))
= (A 0 (A 0 (A 0 (A 1 7))))
... this will continue on until
= (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))
```
Since `(A 1 1)` is 2 (from the third line of the `cond`), this is `(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))`.

Then each of those instances of `(A 0 y)` evaluate to `(* 2 y)` and the result is 2<sup>10</sup>. 

We can see that `(A 1 n)` is 2<sup>`n`</sup>, for any positive integer `n` (see the procedure `g` below). We will use this in calculating `(A 2 4)` below.

### (A 2 4)

```scheme
(A 2 4)
= (A 1 (A 2 3))
= (A 1 (A 1 (A 2 2)))
= (A 1 (A 1 (A 1 (A 2 1))))
= (A 1 (A 1 (A 1 2))) ; using the third line of the `cond`
= (A 1 (A 1 4)) ; using the general result from the previous example
= (A 1 16) ; again using the general result from the previous example
```
which is 2<sup>16</sup>, once more using the general result from the previous example.

### (A 3 3)

```scheme
(A 3 3)
= (A 2 (A 3 2))
= (A 2 (A 2 (A 3 1)))
= (A 2 (A 2 2))
= (A 2 (A 1 (A 2 1)))
= (A 2 (A 1 2))
= (A 2 4)
```

This is the same as the previous example, 2<sup>16</sup>.

### (define (f n) (A 0 n))

`(f n)` = `(A 0 n)` = `(* 2 n)` for any positive integer `n`.

### (define (g n) (A 1 n))

See the expansion of `(A 1 10)` above. `(g n)` is `(A 1 n)`, which is 2<sup>`n`</sup>, for any positive integer `n`.

### (define (h n) (A 2 n))

`(h n) = (A 2 n) = (A 1 (A 2 (- n 1)))`.
This is the same as `(g (A 2 (- n 1)))`, or `(g (h (- n 1)))`.

Another way to write this is 2<sup>`(h (- n 1))`</sup>.

