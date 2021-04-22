---
layout: default
---

> [Exercise 1.12](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.12). The following pattern of numbers is called Pascal's triangle.
>
> ![five rows of Pascal's triangle](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/ch1-Z-G-17.gif)
>
> The numbers at the edge of the triangle are all 1, and each number inside the triangle is the sum of the two numbers above it. Write a procedure that computes elements of Pascal's triangle by means of a recursive process.

The procedure `pascal` takes a row number and a column number and returns the entry in Pascal's triangle for that row and column.

```scheme
(define (pascal row column)
  (cond ((= column 1) 1)
        ((= column row) 1)
        (else (+ (pascal (- row 1) (- column 1))
                 (pascal (- row 1) column)))))
```

