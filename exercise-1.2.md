---
layout: default
---

> [Exercise 1.2](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html#%_thm_1.2). Translate the following expression into prefix form
>
> ![big complicated fraction](https://i.imgur.com/AL80BpC.png)

`(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) (* 3 (- 6 2) (- 2 7)))`

Or you can space it out like this:

```scheme
(/ (+ 5
      4
      (- 2
         (- 3
            (+ 6
               (/ 4 5)))))
    (* 3
       (- 6 2)
       (- 2 7)))
```
