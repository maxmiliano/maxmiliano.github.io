---
title: "A Celestial Secret of Ruby: The Spaceship Operator 🚀"
subtitle: "Blasting off with Ruby's Retro-Styled Comparison Tool"
author: Max
tags:
  - Ruby
  - programming
  - code
  - Quick Tip
  - SeQura
  - Storytelling
comments: true
related: true
date: 2023-10-16 08:20 +0200
last_modified_at: 2023-10-16 08:20 +0200
tagline: "Exploring the Mysteries of Ruby's Spaceship"
layout: single
classes: wide
excerpt: "A whimsical exploration into Ruby's spaceship operator, how it harkens back to classic gaming, and how we can use it to compare Jedi midichlorian counts."
header:
  teaser: assets/images/helloMax_a_TIE_Fighter_in_sapce_with_vivid_colors_94032ffd-ac5f-4905-947e-6c027c6b6838.png
  overlay_image: assets/images/helloMax_a_TIE_Fighter_in_sapce_with_vivid_colors_94032ffd-ac5f-4905-947e-6c027c6b6838.png
  overlay_filter: 0.65
---
During a cosmic session of Ruby Tapas with my SeQura colleagues, I unearthed a celestial secret: the Ruby `<=>` operator is affectionately named the "spaceship operator"! 🚀

Curious about its out-of-this-world naming? I believe it hearkens back to the retro gaming era from a galaxy far, far away, where spaceships bore a striking resemblance to these characters joined together.

If you're not familiar with it, this UFO of Ruby can return -1, 0, or 1. Much like a Jedi determining whether the Force is stronger, balanced, or weaker:
~~~ruby
class Jedi
  include Comparable

  attr_accessor :midichlorian_count

  def initialize(midichlorian_count)
    @midichlorian_count = midichlorian_count
  end
  
  # Here we define our spaceship operator for custom comparison
  def <=>(other_jedi)
    self.midichlorian_count <=> other_jedi.midichlorian_count
  end
end

obi_wan = Jedi.new(10000)
anakin = Jedi.new(20000)

# Compare midichlorian counts using the spaceship operator
puts obi_wan <=> anakin  # Output: -1 (Obi-Wan has fewer midichlorians)
puts anakin <=> obi_wan  # Output: 1 (Anakin has more midichlorians)

# Check if one Jedi power is greater than the other's using greater than oeprator
puts obi_wan > anakin    # Output: false (Obi-Wan has fewer midichlorians)
puts anakin > obi_wan    # Output: true (Anakin has more midichlorians)
~~~
Note that though we only implement the `<=>` operator, we can still use other comparison operators, such as `>`, `>=`, `<=`, `<`, and `==`. This is courtesy of the `Comparable` module working togeghet with our newest best friend forever, the `<=>`, aka "spaceship operator".

And from now on, every time I encounter that operator, not only will John Williams’ iconic theme echo in my mind, but pixelated spaceships from classic games will zip-zap-buzz around too. As always, the vast universe of Ruby never ceases to astonish. What's even more curious? I can't even recall what I used to call this operator before.

Until next time, may the spaceship be with you!
