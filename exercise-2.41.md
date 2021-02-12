---
layout: default
---

> [Exercise 2.41](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-15.html#%_thm_2.41).  Write a procedure to find all ordered triples of distinct positive integers i, j, and k less than or equal to a given integer n that sum to a given integer s.

I modeled my answer after [my answer to the previous exercise](https://aelanteno.github.io/sicp-exercises/exercise-2.40), which is

```
(define (prime-sum-pairs n)
  (map make-pair-sum
       (filter prime-sum?
               (unique-pairs n))))
```

For this exercise, I wrote `triple-sum`:

```
(define (triple-sum n s)
  (filter (lambda (seq) (sequence-sum? seq s))
          (unique-triples n)))
```

The first argument, `n`, is the number that the numbers in the triple have to be less than or equal to. The second argument, `s`, is the target sum. `unique-triples`, modeled after `unique-pairs`, finds all ordered triples of distinct positive integers i, j, and k less than or equal to a given integer n. Then `sequence-sum?` is used to filter that list of ordered triples to keep only the ones that add up to the given integer `s`.

Note that an analogue `make-pair-sum` isn’t needed because that was used only to stick the sum onto the end of each ordered pair. For this exercise, we don’t need to stick anything onto the end of the order triple.

### `unique-triples`

Here’s `unique-pairs`:

```
(define (unique-pairs n)
  (flatmap (lambda (i)
             (map (lambda (j) (list i j))
                  (enumerate-interval 1 (- i 1))))
             (enumerate-interval 1 n)))
```

To write `unique-triples`, start with the innermost part of `unique-pairs`, except change the letters and make a triple instead of a pair:

`(map (lambda (k) (list i j k)) (enumerate-interval 1 (- j 1)))`

This produces a list of triples `(i j k)` that start with a given `i` and a given `j`, where 0 < `k` < `j`.

Example:

```
(define i 5)
(define j 4)
(define test1 (map (lambda (k) (list i j k)) (enumerate-interval 1 (- j 1))))

> test1
((5 4 1) (5 4 2) (5 4 3))
```

Next, for a given i, take the sequence representing `j` going from 1 to `i`-1 and `flatmap` it onto the above, to give a list of triples for each `j` with 0 < `j` < `i`:

```
(flatmap (lambda (j)
             (map (lambda (k) (list i j k)) (enumerate-interval 1 (- j 1))))
           (enumerate-interval 1 (- i 1)))
```

Example: 

```
(define i 5)
(define test2
  (flatmap (lambda (j)
             (map (lambda (k) (list i j k)) (enumerate-interval 1 (- j 1))))
           (enumerate-interval 1 (- i 1))))

> test2
((5 2 1) (5 3 1) (5 3 2) (5 4 1) (5 4 2) (5 4 3))
```

The last step is to take the sequence representing `i` going from 1 to `n` and `flatmap` it onto the previous `flatmap`, to give that list of triples for all the `i` from 1 to `n`. Then you have `unique-triples`:

```
(define (unique-triples n)
  (flatmap (lambda (i)
             (flatmap (lambda (j)
                        (map (lambda (k) (list i j k)) (enumerate-interval 1 (- j 1))))
                      (enumerate-interval 1 (- i 1))))
           (enumerate-interval 1 n)))

> (unique-triples 3)
((3 2 1))
> (unique-triples 4)
((3 2 1) (4 2 1) (4 3 1) (4 3 2))
> (unique-triples 5)
((3 2 1) (4 2 1) (4 3 1) (4 3 2) (5 2 1) (5 3 1) (5 3 2) (5 4 1) (5 4 2) (5 4 3))
```

### `sequence-sum?`

`sequence-sum?` is used in this exercise for triples, but I wrote it to be more generally usable for a sequence of any length. The first argument `seq` is a sequence of numbers. The second argument `sum` is the target sum. If the numbers in the sequence add up to the target sum, the procedure returns #t; otherwise it returns #f.

```
(define (sequence-sum? seq sum)
  (equal? sum (accumulate + 0 seq)))

> (sequence-sum? (list 1 2 3) 6)
#t
> (sequence-sum? (list 1 2 3) 5)
#f
> (sequence-sum? (list 1 2 3 4 5 6 7 8) 36)
#t
> (sequence-sum? (list 1 2 3 4 5 6 7 8) 46)
```

### Testing `triple-sum`

```
(define (triple-sum n s)
  (filter (lambda (seq) (sequence-sum? seq s))
          (unique-triples n)))

> (triple-sum 4 6)
((3 2 1))
> (triple-sum 5 7)
((4 2 1))
> (triple-sum 5 8)
((4 3 1) (5 2 1))
```

Other procedures needed to get these to run:

```
(define (enumerate-interval low high)
  (if (> low high)
      nil
      (cons low (enumerate-interval (+ low 1) high))))

(define (flatmap proc seq)
  (accumulate append nil (map proc seq)))

(define (accumulate op initial sequence)
  (if (null? sequence)
      initial
      (op (car sequence)
          (accumulate op initial (cdr sequence)))))

(define (filter predicate sequence)
  (cond ((null? sequence) nil)
        ((predicate (car sequence))
         (cons (car sequence)
               (filter predicate (cdr sequence))))
        (else (filter predicate (cdr sequence)))))
```

### Not needed for this exercise: `unique-tuples`

`unique-tuples` is a general case procedure of `unique-pairs` and `unique-triples`, for tuples of any given length. `n` is the maximum value of the elements of the tuples and `k` is the length of the tuples.

The procedure does recursion on both `k` and `n` at the same time. I assumed that both `n` and `k` are integers.

```
(define (unique-tuples n k)
  (cond ((< k 1) (list nil))
        ((< n 1) nil)
        ((< n k) nil)
        (else (flatmap (lambda (i)
                         (map (lambda (l) (cons i l))
                              (unique-tuples (- i 1) (- k 1))))
                       (enumerate-interval 1 n)))))

> (unique-tuples 3 2)
((2 1) (3 1) (3 2))
> (unique-tuples 2 3)
()
> (unique-tuples 3 0)
(())
> (unique-tuples 0 3)
()
```
