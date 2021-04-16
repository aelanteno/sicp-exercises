---
layout: default
---

> [Exercise 1.8](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.8). Newton's method for cube roots is based on the fact that if y is an approximation to the cube root of x, then a better approximation is given by the value
>
> ![(x/y^2 + 2y)/3](https://i.imgur.com/Mvbsuvk.png)
>
> Use this formula to implement a cube-root procedure analogous to the square-root procedure. (In section [1.3.4](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-12.html#%_sec_1.3.4) we will see how to implement Newton's method in general as an abstraction of these square-root and cube-root procedures.)

I modeled this after the improved procedures written for the previous exercise. The names `cube-root` and `cube-root-iter` are different but the procedures are the same as the analagous ones. The procedure `good-enough?` is the same except the tolerance is smaller for more accuracy. The procedure `improve-guess` has the formula given in this exercise instead of the one for improving square roots.

```scheme
(define (cube-root x)
  (cube-root-iter 0.0 1.0 x))

(define (cube-root-iter previous-guess guess x)
  (if (good-enough? previous-guess guess)
      guess
      (cube-root-iter guess (improve guess x) x)))

(define (good-enough? previous-guess guess)
  (< (abs (/ (- previous-guess guess) guess)) 0.0000001))

(define (improve guess x)
  (/ (+ (/ x (square guess)) (* 2 guess)) 3))

(define (square a)
  (* a a))
```

This works for negative numbers too, with one exception. When the given number is -2, the program gets stuck. To see why, look at the first few iterations:
- The first `good-enough?` returns #f.
- So `(cube-root-iter 1.0 (improve 1.0 -2) -2)` is called.
- `(improve 1.0 -2)` returns 0.0.
- We have `(cube-root-iter 1.0 0.0 -2)`.
- This calls `(good-enough? 1.0 0.0)`.
- I thought this would return an error because it tries to divide by 0. But it turns out it doesn't return an error; it returns `#f`. `(abs (/ (- 1.0 0.0) 0.0))` returns `+inf.0`. And `(< +inf.0 0.0000001)` returns `#f`. So the procedure continues.
- It calls `(cube-root-iter 0.0 (improve 0.0 -2) -2)`.
- Again, `(improve 0.0 -2)` divides by 0, but it doesn't return an error. It returns `-inf.0`.
- We have `(cube-root-iter 0.0 -inf.0 -2)`.
- It checks `(good-enough? 0.0 -inf.0)` and returns `#f` again.
- So it calls `(cube-root-iter -inf.0 (improve -inf.0 -2) -2)`.
- `(improve -inf.0 -2)` evaluates to `-inf.0`.
Now we have an infinte loop. `-inf.0` keeps improving to itself and `good-enough?` keeps returning `#f`.

Here's a log:

```scheme
> (cube-root -2)
>{cube-root-iter 0.0 1.0 -2}
> {good-enough? 0.0 1.0}
< #f
> {improve 1.0 -2}
< 0.0
>{cube-root-iter 1.0 0.0 -2}
> {good-enough? 1.0 0.0}
< #f
> {improve 0.0 -2}
< -inf.0
>{cube-root-iter 0.0 -inf.0 -2}
> {good-enough? 0.0 -inf.0}
< #f
> {improve -inf.0 -2}
< -inf.0
>{cube-root-iter -inf.0 -inf.0 -2}
> {good-enough? -inf.0 -inf.0}
< #f
> {improve -inf.0 -2}
< -inf.0
>{cube-root-iter -inf.0 -inf.0 -2}
> {good-enough? -inf.0 -inf.0}
< #f
```

So I added a special case for if the input is -2:

```scheme
(define (cube-root x)
  (if (= x -2)
      (cube-root-iter 0.0 2.0 x)
      (cube-root-iter 0.0 1.0 x)))
```

Now we get the expected answer for an input of -2:

```scheme
> (cube-root 2)
1.2599210498948732
> (cube-root -2)
-1.2599210498948732
```