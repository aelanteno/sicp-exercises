---
layout: default
---

> [Exercise 1.11](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.11). A function f is defined by the rule that f(n) = n if n<3 and f(n) = f(n - 1) + 2f(n - 2) + 3f(n - 3) if n> 3. Write a procedure that computes f by means of a recursive process. Write a procedure that computes f by means of an iterative process.

### Recursive process

```scheme
(define (recursive-f n)
  (if (< n 3)
      n
      (+ (recursive-f (- n 1))
         (* 2 (recursive-f (- n 2)))
         (* 3 (recursive-f (- n 3))))))
```

### Iterative process

`counter` counts up from 3 to `n`. `a` represents f(`counter` - 3), `b` represents f(`counter` - 2), and `c` represents f(`counter` - 1).

```scheme
(define (iterative-f n)
  (define (iter-f a b c counter)
    (if (= counter n)
        (+ (* 3 a) (* 2 b) c)
        (iter-f b c (+ (* 3 a) (* 2 b) c) (+ counter 1))))
  (if (< n 3)
      n
      (iter-f 0 1 2 3)))
```

**Note 1:** It seems more natural to me to count up to `n` rather than back from `n`, since the process calculates from lower numbers to higher numbers.

**Note 2:** Since the definition of `iter-f` is inside the definition of `interative-f`, we can use `n` inside the definition of `iter-f` without having to pass it in as an argument.