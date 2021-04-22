---
layout: default
---

> [Exercise 1.13](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.13). Prove that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5, where &#632; = (1 + &#8730;5)/2. Hint: Let &#968; = (1 - &#8730;5)/2. Use induction and the definition of the Fibonacci numbers (see section [1.2.2](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2.2)) to prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

Outline of the proof:
1. Prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.
    - Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 0.
    - Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 1.
    - Show that for any n &#8805; 2, **if** Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5 **and** Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5, **then** Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.
    - Conclude that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 for any n &#8805; 0.
2. Use 1 to show that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5.

### 1. Prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

#### Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 0

When n = 0,
(&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5
= (&#632;<sup>0</sup> - &#968;<sup>0</sup>)/&#8730;5
= (1 - 1)/&#8730;5
= 0
= Fib(0).

#### Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 1

When n = 1,
(&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5
= (&#632; - &#968;)/&#8730;5

![big fraction of phi - psi written out in numbers, all divided by sqrt 5](https://i.imgur.com/XaJkU3C.png)

= ((2&#8730;5)/2)/&#8730;5
= 1
= Fib(1).

#### For n &#8805; 2

Now we'll assume that Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5 and Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5 and show that for n &#8805;, Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 for any n &#8805; 0.

First, note that &#632; * &#968; = -1 and &#632; + &#968; = 1. We'll use these later.

Fib(n)
*from the definition of the Fib numbers*
= Fib(n-1) + Fib(n-2) *from the definition of the Fib numbers*
*from the assumptions above*
![the assumptions written out](https://i.imgur.com/mpGbw8T.png)











 Since all the Fib numbers are built up from previous Fib numbers and ultimately from Fib(0) and Fib(1), 