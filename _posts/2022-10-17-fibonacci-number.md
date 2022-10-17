---
title: Fibonacci Number code challenge
author: Max
tags:
  - Ruby
  - code challenge
  - code
comments: true
related: true
header:
  teaser: assets/images/2022-10-16-Fibonacci_Code_Challenge-thumb.png
---
The Fibonacci Number is another common code challenge and it has some simililraties with the Factorial Number we already solved.

In this videos I show three ways to implement a solution for this problem following similar approaches: iterative loop, recursion and using `inject` (which is also iterative, but it's cooler). 
{% include video id="SBow7lLfI1Q" provider="youtube" %}

Iterative (`upto` loop):
~~~ruby
def fibo(n)
  seq = [0, 1]
  2.upto(n).each do |i|
    set[i] = seq[i-1] + seq[i-2]
  end
  seq[n]
end
~~~

Recursive:
~~~ruby
def fibo(n)
  return n if n <= 1
  fibo(n-1) + fibo(n-2)  
end
~~~

Using `inject`:
~~~ruby
def fibo(n)
  return n if n <= 1
  (2..n).inject([0,1]){|fib| fib << fib[-1] + fib[-2]}[n]
end
~~~
