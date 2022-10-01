---
title: Hello, World!
subtile: "Does it make sense writing 'Hello, World!' as my first post?"
author: Max
tags: [ruby, helloWorld, code]
comments: true
layout: post
---

## Welcome

**Hello World**, this my first blog post.

~~~ruby
class HelloWorld
   def initialize(name)
      @name = name.capitalize
   end
   def say
      puts "Hello #{@name}!"
   end
end

hello = HelloWorld.new("Max")
hello.say
~~~
<!--more-->
