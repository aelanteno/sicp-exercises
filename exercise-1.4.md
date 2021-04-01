---
layout: default
---

> [Exercise 1.4](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.4). Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:
>
>     (define (a-plus-abs-b a b)  
>       ((if (> b 0) + -) a b))  

This procedure applies an operator to `a` and `b`. That operator is `(if (> b 0) + -)`. If `b` is greater than 0, the operator evaluates to `+`. If not, it evaluates to `-`.

The overall effect is to return `a` plus the absolute value of `b`.