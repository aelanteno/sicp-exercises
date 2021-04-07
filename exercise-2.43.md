---
layout: default
---

> [Exercise 2.43](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-15.html#%_thm_2.43). Louis Reasoner is having a terrible time doing exercise [Exercise 2.42](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-15.html#%_thm_2.42). His `queens` procedure seems to work, but it runs extremely slowly. (Louis never does manage to wait long enough for it to solve even the 6× 6 case.) When Louis asks Eva Lu Ator for help, she points out that he has interchanged the order of the nested mappings in the `flatmap`, writing it as
>
```scheme
> (flatmap
>  (lambda (new-row)
>    (map (lambda (rest-of-queens)
>           (adjoin-position new-row k rest-of-queens))
>         (queen-cols (- k 1))))
>  (enumerate-interval 1 board-size))
```
>
> Explain why this interchange makes the program run slowly. Estimate how long it will take Louis's program to solve the eight-queens puzzle, assuming that the program in exercise [Exercise 2.42](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-15.html#%_thm_2.42) solves the puzzle in time *T*.

The original `queens` procedure in Exercise 2.42 is:

```scheme
(define (queens board-size)
  (define (queen-cols k)  
    (if (= k 0)
        (list empty-board)
        (filter
         (lambda (positions) (safe? k positions))
         (flatmap
          (lambda (rest-of-queens)
            (map (lambda (new-row)
                   (adjoin-position new-row k rest-of-queens))
                 (enumerate-interval 1 board-size)))
          (queen-cols (- k 1))))))
  (queen-cols board-size))
```

Compare the two `flatmap` sections.

The `flatmap` section from the original:

```scheme
(flatmap
  (lambda (rest-of-queens)
    (map (lambda (new-row)
           (adjoin-position new-row k rest-of-queens))
          (enumerate-interval 1 board-size)))
  (queen-cols (- k 1)))
```

Louis’ `flatmap` section:

```scheme
(flatmap
  (lambda (new-row)
    (map (lambda (rest-of-queens)
           (adjoin-position new-row k rest-of-queens))
         (queen-cols (- k 1))))
  (enumerate-interval 1 board-size))
```

Note that this `flatmap` section is part of the `queen-cols` procedure. In both versions, it calls itself. The important difference is in how many times it calls itself. In the original version, each time `flatmap` runs, `queen-cols` is called once. In Louis’ version, each time `flatmap` runs, `queen-cols` is called once for each element of `(enumerate-interval 1 board-size)`, which is *B* times, where *B* is the size of the board, which is given to the procedure by the argument `board-size`.

In the original `queens` procedure, `queen-cols` gets called *B* times. `k` starts at *B* and is decreased by 1 each time until it’s 0. I’m not counting when `queen-cols` is called with `k` = 0, because it doesn’t do much then.

In Louis’ `queens` procedure, 

- `queen-cols` gets called once with `k` = *B*.
- That calls `queen-cols` again with `k` = *B* - 1, *B* times.
- Each of those calls with `k` = *B* - 1 calls `queen-cols` with `k` = *B* - 2 *B* times, for a total of *B*<sup>2</sup> times that `queen-cols` gets called with `k` = *B* - 2.
- Similarly, each of the calls to `queen-cols` with `k` = *B* - 2 calls `queen-cols` with `k` = *B* - 3 *B* times, for a total of *B*<sup>3</sup> times that `queen-cols` gets called with `k` = *B* - 3.
- Continuing, we get to the case where `k` = 1 (again, I’m not counting the cases where `queen-cols` is called with `k` = 0, because `queen-cols` hardly does anything then). `k` = 1 is the same as *k* = *B* - (*B* -1), so `queen-cols` gets called with `k` = 1 a total of *B*<sup>(*B* - 1)</sup> times.

Adding up all these times that `queen-cols` gets called, we get *B* + *B*<sup>2</sup> + *B*<sup>3</sup> + ... + *B*<sup>(*B* - 1)</sup>. However, when `queen-cols` is called those *B*<sup>(*B* - 1)</sup> times with `k` = 1, it doesn’t take that much time for each one because `(queen-cols 0)` is `nil` so Louis’ procedure doesn’t have much to do--it just `adjoin-positions` each of the *B* `new-row`s with `nil`.
