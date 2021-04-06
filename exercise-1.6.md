---
layout: default
---

> [Exercise 1.6](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.6). Alyssa P. Hacker doesn't see why `if` needs to be provided as a special form. "Why can't I just define it as an ordinary procedure in terms of `cond`?" she asks. Alyssa's friend Eva Lu Ator claims this can indeed be done, and she defines a new version of `if`:
> 
>     (define (new-if predicate then-clause else-clause)
>       (cond (predicate then-clause)
>             (else else-clause)))
> 
> Eva demonstrates the program for Alyssa:
> 
>     (new-if (= 2 3) 0 5)
>     5
> 
>     (new-if (= 1 1) 0 5)
>     0
> 
> Delighted, Alyssa uses `new-if` to rewrite the square-root program:
> 
>     (define (sqrt-iter guess x)
>       (new-if (good-enough? guess x)
>               guess
>               (sqrt-iter (improve guess x)
>                          x)))
> 
> What happens when Alyssa attempts to use this to compute square roots? Explain.

When Alyssa uses `new-if` in the `sqrt-iter` procedure, it gets stuck in an infinite loop.

Because Scheme interpreters use applicative order evaluation, when `new-if` is called for the first time, the interpreter tries to evaluate all of its arguments. One of its arguments is `(sqrt-iter (improve guess x) x)`, so `sqrt-iter` is called again, with different arguments. Nothing stops `sqrt-iter` from being called over and over in an infinite loop, even when the guess is good enough, because the interpreter tries to evaluate `sqrt-iter (improve guess x) x)` every time.

If the interpreter was using normal order evaluation, it would not get stuck in an infinite loop. When it got to an iteration where the `good-enough?` test was `#t`, it wouldn’t try to evaluate `(sqrt-iter (improve guess x) x)` because it wouldn’t need to in order to evaluate that `new-if`.
