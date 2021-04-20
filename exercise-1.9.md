---
layout: default
---

> [Exercise 1.9](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.9). Each of the following two procedures defines a method for adding two positive integers in terms of the procedures inc, which increments its argument by 1, and dec, which decrements its argument by 1.
>
```scheme
> (define (+ a b)
>   (if (= a 0)
>       b
>       (inc (+ (dec a) b))))
>
> (define (+ a b)
>   (if (= a 0)
>       b
>       (+ (dec a) (inc b))))
```
>
> Using the substitution model, illustrate the process generated by each procedure in evaluating (+ 4 5). Are these processes iterative or recursive?

The first procedure generates a recursive process. It calls itself and then does something with the result of that call besides just pass it on. In this case it does `inc` to it. Here's what happens with `(+ 4 5)`:

```scheme
(+ 4 5)
(inc (+ (dec 4) 5))
(inc (+ 3 5))
(inc (inc (+ (dec 3) 5)))
(inc (inc (+ 2 5))
(inc (inc (inc (+ (dec 2) 5))))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ (dec 1) 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8)
9
```

The second procedure generates an iterative process. It calls itself and then just passes along the result of that call. Here's what happens with `(+ 4 5)`:

```scheme
(+ 4 5)
(+ (dec 4) (inc 5))
(+ 3 6)
(+ (dec 3) (inc 6))
(+ 2 7)
(+ (dec 2) (inc 7))
(+ 1 8)
(+ (dec 1) (inc 8))
(+ 0 9)
9
```
