---
layout: default
---

> [Exercise 1.13](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_thm_1.13). Prove that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5, where &#632; = (1 + &#8730;5)/2. Hint: Let &#968; = (1 - &#8730;5)/2. Use induction and the definition of the Fibonacci numbers (see section [1.2.2](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2.2)) to prove that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

Outline of the proof, following the hint:
<ol>
    <li>Using induction, show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.
        <ol type="a">
        <li>Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 0.</li>
        <li>how that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 1.</li>
        <li>Show that for any n &#8805; 2, <strong>if</strong> Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5 <strong>and</strong> Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5, <strong>then</strong> Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.</li>
        <li>Conclude that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 for any n &#8805; 0.</li>
        </ol>
    </li>
    <li>Use 1. to show that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5.
        <ol type="a">
        <li>Show that the absolute value of 1/&#8730;5 is less than 1/2.</li>
        <li>Show that the absolute value of &#968; is less than 1.</li>
        <li>Conclude that the absolute value of &#968;<sup>n</sup>/&#8730;5 is less than 1/2.</li>
        <li>Conclude that the closest integer to Fib(n) + &#968;<sup>n</sup>/&#8730;5, which is &#632;<sup>n</sup>/&#8730;5, as shown in 1., must be Fib(n).</li>
        </ol>
    </li>
</ol>

## 1. Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

### a. Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 0.

When n = 0,

(&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5

= (&#632;<sup>0</sup> - &#968;<sup>0</sup>)/&#8730;5

= (1 - 1)/&#8730;5

= 0

= Fib(0).

### b. Show that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 when n = 1.

When n = 1,

(&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5

= (&#632; - &#968;)/&#8730;5

<img src="https://i.imgur.com/HFrdflM.png" alt="big fraction of phi - psi written out in numbers, all divided by sqrt 5" height="45"/>

<img src="https://i.imgur.com/neu13pp.png" alt="((2sqrt5)/2)/sqrt5" height="45"/>

= 1

= Fib(1).

### c. Show that for any n &#8805; 2, **if** Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5 **and** Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5, **then** Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

Assume that n &#8805; 2, Fib(n-1) = (&#632;<sup>n-1</sup> - &#968;<sup>n-1</sup>)/&#8730;5, and Fib(n-2) = (&#632;<sup>n-2</sup> - &#968;<sup>n-2</sup>)/&#8730;5.  

First, note that &#632; * &#968; = -1 and &#632; + &#968; = 1. We'll use these later.

Fib(n)

*<small>from the definition of the Fib numbers:</small>*

= Fib(n-1) + Fib(n-2)

*<small>from the assumptions above:</small>*

<img src="https://i.imgur.com/6oaQWPC.png" alt="the assumptions written out" height="45"/>

*<small>multiply the top half of the first fraction by &#632; + &#968;, which = 1:</small>*

<img src="https://i.imgur.com/RoHeUeY.png" alt="after multiplying the top half of the first fraction by &#632; + &#968;" height="45"/>

*<small>replace one &#632; * &#968; with -1 in each of the two middle terms of the top half of the first fraction, since &#632; * &#968; = -1:</small>*

<img src="https://i.imgur.com/9NHb1jE.png" alt="after replacing &#632;&#968; with -1 in terms 2 and 3" height="45"/>

*<small>combine the two fractions and cancel some terms:</small>*

<img src="https://i.imgur.com/i9jChXe.png" alt="(phi ^ n - psi n)/ sqrt 5" height="45"/>

So Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5.

### d. Conclude that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 for any n &#8805; 0.

If the positive integer is 0 or 1, we've already seen that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 is true. If the positive integer is 2, we can use the fact that for any n &#8805; 2, if Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5 is true for n - 1 and n - 2, then it's true for n, because it's true for 0 and 1. Once we know it's true for n = 2, can do the same to show that it's true for n = 3. We can continue doing that until we got to any particular positive number.

## 2. Use 1. to show that Fib(n) is the closest integer to &#632;<sup>n</sup>/&#8730;5.

### a. Show that the absolute value of 1/&#8730;5 is less than 1/2.

1/&#8730;5 is around 0.4472, which has absolute value less than 1/2.

### b. Show that the absolute value of &#968; is less than 1.

&#968;, which is (1 - &#8730;5)/2, is around -0.6180, which has absolute value less than one.

### c. Conclude that the absolute value of &#968;<sup>n</sup>/&#8730;5 is less than 1/2.

For positive integers n, we get to &#968;<sup>n</sup>/&#8730;5 by multiplying 1/&#8730;5 by &#968;, n times. If we start with something with absolute value less than 1/2 and multiply it n times by something with absolute value less than 1, the result will still have absolute value less than 1/2.

### d. Conclude that the closest integer to Fib(n) + &#968;<sup>n</sup>/&#8730;5, which is &#632;<sup>n</sup>/&#8730;5, as shown in 1., must be Fib(n).

We know that Fib(n) is an integer, since Fib numbers are made by adding integers. And from 1., we know that Fib(n) = (&#632;<sup>n</sup> - &#968;<sup>n</sup>)/&#8730;5, which is the same as &#632;<sup>n</sup>/&#8730;5 - &#968;<sup>n</sup>/&#8730;5. Since the second part of that has absolute value less than 1/2, Fib(n) must be the closest integer to the first part of it, which is &#632;<sup>n</sup>/&#8730;5.

