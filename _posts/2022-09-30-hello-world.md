---
title: Hello World!
subtile: "Does it make sense writing 'Hello, World!' as my first post?"
author: Max
tags: 
  - Ruby 
  - hello_world 
  - code
comments: true
related: true
header:
  teaser: assets/images/hello-world-ruby-code.png
  overlay_image: assets/images/hello-world-ruby-code.png
  overlay_filter: 0.65
---

**Hello there**, this my first blog post.

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
