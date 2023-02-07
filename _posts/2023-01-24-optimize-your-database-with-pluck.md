---
title: "Optimize Your Database Queries with pluck"
author: Max
tags:
  - Ruby
  - Rails
  - ActiveRecord
  - Quick Tip
categories:
  - Tips
comments: true
related: true
date: 2023-01-24 08:56 -0300
layout: single
classes: wide
excerpt: "This is a quick tip to help you improving your database queries with the **pluck** ActiveRecord method"
header:
  teaser: assets/images/hand_plucking_a_red_apple_from_a_tree_stylish_ultra_de_21e7fce2-9fb3-428f-b1cb-7cd328a02f08.png
  overlay_image: assets/images/hand_plucking_a_red_apple_from_a_tree_stylish_ultra_de_21e7fce2-9fb3-428f-b1cb-7cd328a02f08.png
  overlay_filter: 0.65
---

Do you ever find yourself needing to grab specific data from your database without the need for instantiating entire Active Record objects? Maybe you just new one or another attribute and that's it. Well, have no fear, because the `pluck` method is here to save the day!

Using `pluck`, you can easily retrieve specific columns from your database without the overhead of creating full Active Record objects. This can be a huge time-saver and optimization for your application.

Here's an example of how to use `pluck` with a model called `Destination` that has the these fields: `id`, `name` and `description`, all of them of type `String`. Check it out:

~~~ruby
# This will retrieve an array of all the destination names in the table
names = Destination.pluck(:name)

# This will retrieve an array of arrays, where each inner array contains the `id`, `name`, and `description` of a destination
destination_attributes = Destination.pluck(:id, :name, :description)
~~~

So the next time you find yourself needing to quickly retrieve specific data from your database, give `pluck` a try! 

Happy coding :)
