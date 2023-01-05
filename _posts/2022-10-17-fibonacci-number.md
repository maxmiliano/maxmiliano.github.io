---
title: Fibonacci Number code challenge
author: Max
tags:
  - Ruby
  - code challenge
  - code
  - YouTube
comments: true
related: true
tagline: "*0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144*"
header:
  teaser: assets/images/2022-10-16-Fibonacci_Code_Challenge-thumb.png
  overlay_image: assets/images/2022-10-16-Fibonacci_Code_Challenge-thumb.png
  overlay_filter: 0.65
---
The Fibonacci Number is another common code challenge and it has some simililraties with the Factorial Number we already solved.

In this videos I show three ways to implement a solution for this problem following similar approaches: iterative loop, recursion and using `inject` (which is also iterative, but it's cooler). 
{% include video id="SBow7lLfI1Q" provider="youtube" %}

1. Iterative (`upto` loop):
~~~ruby
def fibo(n)
  seq = [0, 1]
  2.upto(n).each do |i|
    seq[i] = seq[i-1] + seq[i-2]
  end
  seq[n]
end
~~~

1. Recursive:
~~~ruby
def fibo(n)
  return n if n <= 1
  fibo(n-1) + fibo(n-2)  
end
~~~

1. Using `inject`:
~~~ruby
def fibo(n)
  return n if n <= 1
  (2..n).inject([0,1]){|fib| fib << fib[-1] + fib[-2]}[n]
end
~~~
