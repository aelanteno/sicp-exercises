---
layout: default
---

> [Exercise 2.42](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-15.html#%_thm_2.42).  
>
> ![](https://i.imgur.com/sv9sz8p.png)
>
> Figure 2.8:  A solution to the eight-queens puzzle.
>
> The “eight-queens puzzle” asks how to place eight queens on a chessboard so that no queen is in check from any other (i.e., no two queens are in the same row, column, or diagonal). One possible solution is shown in figure 2.8. One way to solve the puzzle is to work across the board, placing a queen in each column. Once we have placed *k* - 1 queens, we must place the *k*th queen in a position where it does not check any of the queens already on the board. We can formulate this approach recursively: Assume that we have already generated the sequence of all possible ways to place *k* - 1 queens in the first *k* - 1 columns of the board. For each of these ways, generate an extended set of positions by placing a queen in each row of the kth column. Now filter these, keeping only the positions for which the queen in the *k*th column is safe with respect to the other queens. This produces the sequence of all ways to place *k* queens in the first *k* columns. By continuing this process, we will produce not only one solution, but all solutions to the puzzle.
We implement this solution as a procedure `queens`, which returns a sequence of all solutions to the problem of placing *n* queens on an *n* × *n* chessboard. `Queens` has an internal procedure `queen-cols` that returns the sequence of all ways to place queens in the first *k* columns of the board.
>
>(define (queens board-size)   (define (queen-cols k)       (if (= k 0)         (list empty-board)         (filter          (lambda (positions) (safe? k positions))          (flatmap           (lambda (rest-of-queens)             (map (lambda (new-row)                    (adjoin-position new-row k rest-of-queens))                  (enumerate-interval 1 board-size)))           (queen-cols (- k 1))))))   (queen-cols board-size))
>
> In this procedure `rest-of-queens` is a way to place *k* - 1 queens in the first *k* - 1 columns, and *new-row* is a proposed row in which to place the queen for the *k*th column. Complete the program by implementing the representation for sets of board positions, including the procedure `adjoin-position`, which adjoins a new row-column position to a set of positions, and `empty-board`, which represents an empty set of positions. You must also write the procedure `safe?`, which determines for a set of positions, whether the queen in the *k*th column is safe with respect to the others. (Note that we need only check whether the new queen is safe -- the other queens are already guaranteed safe with respect to each other.)

## Choosing a data structure

We have a choice here of what kind of data structure to use. I chose to represent each board position as a list. The first element in the list represents which row the queen is in in the first column of the board. The second element in the list represents which row the queen is in in the second column. Etc. The last element in the list represents which row the queen is in in the last column. The number of elements in the list is the same as the number of columns on the board, which is the `board-size` argument in the given procedure. 

The procedure `queens` returns a list of all the legal board positions for a given `board-size`. So it’s a list of sub-lists, with each sublist being of length `board-size`. 

Example: The position in Figure 2.8, which is a solution for `board-size` = 8, is `(3 7 2 8 5 1 4 6)`. That would be one of the lists in the full set of solutions for `board-size` = 8.

## Understanding the given procedure

### What is `empty-board`?

It’s our job to define `empty-board`. When `k` = 0, the procedure returns `(list empty-board)`. `k` = 0 means there are 0 columns. What are the ways we can arrange 0 queens in 0 columns? There aren’t any. So we define `empty-board` as `nil`:

`(define empty-board nil)`

### What does `queen-cols` do?

`queen-cols` is a procedure that returns a list of all the safe ways to place queens in the first `k` columns of the board.

Example: If `board-size` is 4 and `k` is 2, then `queen-cols` is `((1 3) (1 4) (2 4) (3 1) (4 2) (4 1))`.

### What does `rest-of-queens` represent?

`rest-of-queens` is used as an argument to a `lambda`. It is a list of length `k`-1, representing one way to place `k` - 1 queens in the first `k` - 1 columns.

Example: If `board-size` is 4 and `k` is 3, then `rest-of-queens` could be `(1 4)`.

### What does `new-row` represent?

`new-row` is used as an argument to a different `lambda`. It is a number representing a proposed row in which to place the queen for the `k`th column.

Example: If `board-size` is 4, then `new-row` could be 2.

### What does `adjoin-position` do?

It’s our job to define `adjoin-position`. It’s a procedure that adjoins a new position (row number in my representation) to a set of positions (list of row numbers in my representation). Its arguments are a number `new-row` representing a possible row in which to place the queen in the current (`k`th) column, a number `k` representing the current column, and a list `rest-of-queens` representing one way to legally place queens in `k` - 1 columns.

Example: If the row is 2, the column is 3, and `rest-of-queens` is `(1 4)`, then `adjoin-position` returns `(1 4 2)`.

Using this description, we see that we want `adjoin-position` to stick `new-row` onto the end of `rest-of-queens`. We can use `append` for this, after making `new-row` into a list because `append` expects lists as arguments.

In the definition below of `adjoin-position`, I changed the names of the arguments from those used in `queens` so as not to use the same argument names in two different procedures.

(define (adjoin-position row column position-so-far)
  (append position-so-far (list row)))

Note that `column` isn’t used in the definition. I kept it in there so I could use the `queens` procedure as written in the exercise.

### What does the `map` do?

```scheme
(map (lambda (new-row) (adjoin-position new-row k rest-of-queens))
             (enumerate-interval 1 board-size))
```

This maps the enumerated interval from 1 to `board-size` onto a list of length `board-size`. Each thing in the new list is a sub-list of length `k`. The sub-lists of length `k` all start with the same `k` - 1 entries that were in `rest-of-queens` and each has a different last entry, from 1 to `board-size`.

Example: If `board-size` is 4 and `rest-of-queens` is `(1 3), then this `map` returns `((1 3 1) (1 3 2) (1 3 3) (1 3 4))`.

Note that the positions returned by this `map` may or may not be legal board positions.

### What does the `flatmap` do?

```scheme
(flatmap
  (lambda (rest-of-queens)
    (map (lambda (new-row)
           (adjoin-position new-row k rest-of-queens))
         (enumerate-interval 1 board-size)))
  (queen-cols (- k 1)))
```

This maps all the solutions for `k` - 1 columns onto lists of potential solutions for `k` columns and then flattens the list into one long list.

In other words, it combines the results of `map` for all the solutions already found for `k` - 1 columns.

### What does `safe?` do?

It’s our job to define `safe?`. It’s used in the filter to choose the legal solutions from the potential solutions produced by the `flatmap`.

``safe?` takes two arguments: the number of columns and a list representing some queen positions. That list will be of length `k`. We know that the first `k` - 1 positions in the list are safe together, but we don’t know if the `k`th number in the list is safe with the others and that’s what we need `safe?` to find out.

There are two things to check for:

- Is the last queen in the same row as another queen? In other words, is the last number in the list the same as any of the other numbers in the list?

- Is the last queen on a diagonal with another queen? In other words, is the absolute value of the difference between the last number and any of the other numbers the same as the absolute value of the difference in their positions in the list? 

I wrote a `last` procedure. It returns the last item in a list. That’s the one we want to check against the others. I used `let` so that the last item would only have to be pulled out once for each time `safe?` is used.

I didn’t make use of the `column` argument to `safe?`. Instead, in the helper procedure, I used the length of the list representing the current partial set of positions. I could remove `column` as an argument to `safe?` and change the `(lambda (positions) (safe? k positions))` line to say just `safe?`. I didn’t, in order to fit my answer into the `queens` procedure given in the exercise.

```scheme
(define (last items)
  (if (equal? (cdr items) nil)
      (car items)
      (last (cdr items))))

(define (safe? column position)
  (let ((last-queen-position (last position)))
    (define (safe?-helper partial-position) 
      (cond ((equal? (cdr partial-position) nil) #t)
            ((= (car partial-position) last-queen-position) #f)
            ((= (- (length partial-position) 1) (- last-queen-position (car partial-position))) #f)
            ((= (- (length partial-position) 1) (- (car partial-position) last-queen-position)) #f)
            (else (safe?-helper (cdr partial-position)))))
    (safe?-helper position)))
```

## Other procedures that are needed to get `queens` to run, from the book

(define (flatmap proc seq)
  (accumulate append nil (map proc seq)))

(define (filter predicate sequence)
  (cond ((null? sequence) nil)
        ((predicate (car sequence))
         (cons (car sequence)
               (filter predicate (cdr sequence))))
        (else (filter predicate (cdr sequence)))))

(define (enumerate-interval low high)
  (if (> low high)
      nil
      (cons low (enumerate-interval (+ low 1) high))))

(define (accumulate op initial sequence)
  (if (null? sequence)
      initial
      (op (car sequence)
          (accumulate op initial (cdr sequence)))))

## Testing of the `queens` procedure with all the definitions filled in

```
> (queens 1)
((1))
> (queens 2)
()
> (queens 3)
()
> (queens 4)
((2 4 1 3) (3 1 4 2))
> (queens 5)
((1 3 5 2 4)
 (1 4 2 5 3)
 (2 4 1 3 5)
 (2 5 3 1 4)
 (3 1 4 2 5)
 (3 5 2 4 1)
 (4 1 3 5 2)
 (4 2 5 3 1)
 (5 2 4 1 3)
 (5 3 1 4 2))
> (queens 6)
((2 4 6 1 3 5) (3 6 2 5 1 4) (4 1 5 2 6 3) (5 3 1 6 4 2))
```

`(queens 7)` and `(queens 8)` look correct too, but they take up more space so I didn’t copy them here. `(queens 8)` has 92 solutions and does include `(3 7 2 8 5 1 4 6)`, the solution shown in Figure 2.8.

`(queens 11)` took about 10 seconds to run. I didn’t try higher input than that.

## Other solutions

atomik’s answer on [this page](http://community.schemewiki.org/?sicp-ex-2.42) used the same representation I did. I realized from reading that answer that I could have stuck the new column onto the front of the list instead of the end. Then I wouldn’t have needed to `append` in `adjoin-position` and then I wouldn’t have needed to pull out the last entry using my `last` procedure.

I started to re-write my procedures to do it this way, but then I realized that I’d need a more complicated way of determining if a new queen was on a diagonal with an existing queen. I decided to go with my answer as is. 

Several other answers used a pair of coordinates to represent each queen instead of a single number. The one I like the best is around the middle of [this page](https://ivanovivan.wordpress.com/category/project-sicp/chapter-2/page/2/). 

I especially like what they said about checking diagonals:

> The most interesting one of them is `same-diagonal?`. It uses the fact that for all squares located on the same diagonal either *row* + *col* or *row* – *col* is a constant.
