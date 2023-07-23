---
title: "What Happens When You Run 'rails new my-amazing-app'"
subtitle: "A Dive into the Mysterious World of Rails Generators"
author: Max
tags:
  - Ruby
  - Rails
  - Software Engineer
  - generators
categories:
  - Tutorial
comments: true
related: true
layout: single
classes: wide
date: 2023-07-24 07:20 +0200
last_modified_at: 2023-07-24 07:20 +0200
excerpt: "Ever wondered what exactly happens under the hood when you type 'rails new my-amazing-app'? This post explores the files and directories that Rails creates for you when you set out to build a new Rails application."
toc: false
tagline: "'rails new my-amazing-app' - the magic spell that creates a whole universe for you to explore and play with!"
header:
  teaser: assets/images/HelloMax-10_year_old_kid_birthday_party_447662f5-e9d6-4670-8bea-5126ea5a4b55.png
  overlay_image: assets/images/HelloMax-10_year_old_kid_birthday_party_447662f5-e9d6-4670-8bea-5126ea5a4b55.png
  overlay_filter: 0.65
---
Hey there, fellow Rails enthusiasts! Following up on my previous post about the joy of starting a new Rails app, I thought it'd be cool to dive a little into what exactly happens when you type that magical command `rails new my-amazing-app` into your terminal.

When you run `rails new my-amazing-app`, Rails starts creating a bunch of directories and files in your current directory. It's like your favorite uncle showing up at your 10 year old birthday party with a piñata full of surprises. Each directory and file is like a special gift waiting to be unwrapped. The piñata itself is the root folder `my_amazing_app`.

The first gift is the **app** directory. This is where the majority of our application will be written. It contains subdirectories for models, views, and controllers - the core components of the MVC architecture, as well as helpers, mailers, and assets.

Speaking of MVC (Model-View-Controller), it's like the brain, eyes, and hands of our application. The **models** directory is where we define the data structure, business logic, and rules of our application. It's the brain, you might say. The **views** directory, on the other hand, is what your application 'shows' to the user - it's the eyes. And the **controllers**? They're the hands of the application, directing traffic, processing requests from the user and serving responses, typically rendering a view.

The next gift is the **db** directory. This is where Rails will store your current database schema and the database migration files.

Then we have the **config** directory, which is crucial for your application's environment settings. From your application's routing to database configuration, all can be managed from within this directory.

The **bin** directory contains scripts that are necessary to bundle, rake, and start your Rails app. In short, Rails uses this directory to handle various tasks related to your application.

Next up is the **Gemfile** and **Gemfile.lock**. The Gemfile is where you specify the gem dependencies needed for your Rails application, while Gemfile.lock is where Bundler records the specific versions of each gem that your application uses.

And finally, we have the **README.md**, the friendly introduction to your application. It's a great place to put notes about your app and how to get it up and running.

So, that was a quick tour of what happens when you run `rails new my_amazing_app`. It's pretty amazing, isn't it? This is just the beginning, though. In the next article, I'm going to dive even deeper into the `rails new` command to show you how you can customize your new Rails app. Stay tuned!

Remember, no matter how many times you do it, starting a new Rails app is always a thrilling adventure. The world is your amusement park, and your text editor is your ticket. Hop on the roller coaster and let Rails take you on a thrilling ride!

As always, keep coding and exploring, and don't hesitate to share your experiences and feedback in the comments section below.

Happy coding!

