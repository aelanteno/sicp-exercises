---
layout: default
---

> [Exercise 1.13](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.13). Prove that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5, where &#632; = (1 + &#8730;5)/2. Hint: Let &#968; = (1 - &#8730;5)/2. Use induction and the definition of the Fibonacci numbers (see section [1.2.2](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2.2)) to prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

Outline of the proof:
1. Prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.
    a. Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 0.
    b. Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 1.
    c. Show that for any n >= 2, **if** Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5 **and** Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5, **then** Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.
2. Use 1 to show that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5.