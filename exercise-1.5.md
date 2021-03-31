---
layout: default
---

> [Exercise 1.5](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.5). Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:
>
>    (define (p) (p))  
>
>    (define (test x y)  
>      (if (= x 0)  
>          0  
>          y))  
>
> Then he evaluates the expression
>
>    (test 0 (p))  
>
> What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer. (Assume that the evaluation rule for the special form if is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate the consequent or the alternative expression.)

The procedure `p` returns itself. So when you call it, the interpreter needs to evaluate `(p)` in order to evaluate `(p)`. It gets stuck in an infinite loop, trying over and over to evaluate `(p)`.

Using applicative-order evaluation, the interpreter evaluates the operator and operands before it applies the operator to the operands. So when you input `(test 0 (p))`, the interpreter first tries to evaluate `(p)` before applying `test` to `0` and `(p)`. It gets stuck in a loop while trying to evaluate `(p)` and never gets to `test`.

Using normal-order evaluation, the interpreter doesn't evaluate the operator and operands until it needs to. So when you input `(test 0 (p))`, it applies `test` according to the instructions in the definition of `test`. It gets to the second line and substitutes `0` in for `x` and evaluates `(if (= 0 0))` to true. So it goes to the third line and returns `0`. It doesn’t need to evaluate `(p)` because it never gets to the fourth line. Since it never tries to evaluate `(p)`, it doesn't get stuck on it.