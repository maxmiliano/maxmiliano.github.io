---
title: "Hello, World!
date: 2022-09-30
---

# Welcome

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
