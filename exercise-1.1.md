---
layout: default
---

> [Exercise 1.1](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.1).  Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.
>
```scheme
> 10
```

10

>
```scheme
> (+ 5 3 4)
```
>

12

```scheme
> (- 9 1)
```

8

```scheme
> (/ 6 2)
```

3

```scheme
> (+ (* 2 4) (- 4 6))
```

6

```scheme
> (define a 3)
```

The interpreter doesn't respond to definitions, but it remembers them. In everything below, it will evaluate `a` to 3.

```scheme
> (define b (+ a 1))
```

Again, the interpreter doesn't respond. But in everything below, it will evaluate `b` to 4.

```scheme
> (+ a b (* a b))
```

19

```scheme
> (= a b)
```

#f

```scheme
> (if (and (> b a) (< b (* a b)))  
>     b  
>     a)
```

4

```scheme
> (cond ((= a 4) 6)` 
>       ((= b 4) (+ 6 7 a))  
>       (else 25))
```

16

```scheme
> (+ 2 (if (> b a) b a))
```

6

```scheme
> (* (cond ((> a b) a)  
>          ((< a b) b)  
>          (else -1)) 
>    (+ a 1))
```

16