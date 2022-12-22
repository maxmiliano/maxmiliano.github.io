---
title: Adding Authentication to a Rails App with Devise
author: Max
tags:
  - Ruby
  - Rails
  - code
  - gems
  - programming
  - identity
comments: true
related: true
date: 2023-01-10 09:02 -0300
toc: true
toc_sticky: true
toc_label: "Authentication with Devise"
header:
  teaser: assets/images/padlock-g60c73fc6e_1920.jpg
---

In this tutorial, we will be adding authentication to a Rails app using the Devise library. Devise is a popular gem that provides a full-featured and flexible authentication solution for Rails apps.

## Step 1: Install the Devise gem
Add the Devise gem to your Gemfile and run the `bundle install` command to install it.

~~~ruby
# Gemfile
gem 'devise'
~~~

~~~sh
bundle install
~~~

## Step 2: Run the Devise installation generator
Run the Devise installation generator to create the necessary files and configuration for Devise.

~~~sh
rails generate devise:install
~~~

This generator will create a `config/initializers/devise.rb`  file and add a few lines of code to your `config/routes.rb` file. It will also provide you with instructions for setting up Devise in your app, such as setting the default URL options and enabling flash messages.

## Step 3: Create a Devise model
Run the Devise model generator to create a model for your user. If you already a model that represents a user (such as `Person`, `Traveller` or any other) just replace `User` with the name of your model. In this article I'll just stick to `User`.

~~~sh
rails generate devise User
~~~

This generator will create a User model and a migration file to add the necessary database columns for Devise.

## Step 4: Run the Devise migration
Run the migration to create the necessary database columns for Devise.

~~~sh
rails db:migrate
~~~

## Step 5: Add Devise views
If you want to customize the views for Devise (e.g., the login and registration forms), you can generate the Devise views and customize them as needed editingi.

~~~sh
rails generate devise:views
~~~

## Step 6: Add Devise routes
Add the Devise route to your `config/routes.rb` file.

~~~ruby
# config/routes.rb

Rails.application.routes.draw do

  # Add the line below:
  devise_for :users
end
~~~

## Step 7: Add authentication to your controllers and views
To require authentication for certain actions in your controllers, use the `authenticate_user!` method. For example:

~~~ruby
class DestinationsController < ApplicationController
  before_action :authenticate_user!, only: [:create, :update, :destroy]

  def index
    # display a list of destinations
  end

  def show
    # display a single destination
  end

  def create
    # create a new destination
  end

  def update
    # update an existing destination
  end

  def destroy
    # delete a destination
  end
end
~~~

In this example, we are requiring authentication for the `create`, `update`, and `destroy` actions in the DestinationsController. Only authenticated users will be able to access these actions. The `index` and `show` actions will still be available for unauthenticated user.

To display a login or logout link in your views, you can use the `current_user` and `user_signed_in?` helper methods provided by Devise. The `current_user` method returns the currently logged-in user, and the `user_signed_in?` method returns a boolean indicating whether or not a user is signed in.

For example, you can use the following code in a view file to display a login link if the user is not signed in, or a welcome message and logout link if the user is signed in:

~~~erb
<% if user_signed_in? %>
  Welcome, <%= current_user.email %>!
  <%= link_to "Log out", destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to "Log in", new_user_session_path %>
<% end %>
~~~

## Conclusion
In this tutorial, we learned how to add authentication to a Rails app using the Devise library. We installed the Devise gem, ran the Devise installation generator, created a Devise model, ran the Devise migration, added Devise views and routes, and implemented authentication in our controllers and views.

With Devise, it is easy to add a full-featured and flexible authentication system to your Rails app. Whether you need simple authentication for a personal project or advanced features for a large-scale application, Devise has you covered.

I hope this tutorial was helpful in getting you started with Devise. If you have any questions or comments, please don't hesitate to reach out.

Happy coding!
