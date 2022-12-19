---
title: Get Started with Ruby on Rails and Create a Travel CRUD
author: Max
tags:
  - Ruby
  - computer science
  - code
  - CRUD
  - programming
comments: true
related: true
header:
  teaser: assets/images/annie-spratt-qyAka7W5uMY-unsplash.jpg
---

Ruby on Rails is a fantastic web development framework that makes it easy to build web applications with powerful features. In this article, we'll show you how to create a simple CRUD (create, read, update, and delete) web application for a travel domain using Ruby on Rails, and explain why it's a great choice for beginner programmers.

One of the best things about using Ruby on Rails for a CRUD web application is that it comes with a built-in ORM (Object-Relational Mapping) called Active Record. This ORM allows you to define your data models and interact with the database using simple, intuitive commands, without having to write complex SQL queries or deal with the low-level details of the database. This makes it much easier for beginners to manage and work with the data in their application.

Another benefit of using Ruby on Rails is that it follows a set of best practices and conventions for structuring your application, known as "convention over configuration". This means that Rails provides a standard way of organizing your code and files, which can help beginners avoid making common mistakes and ensure that their code is maintainable and scalable. For example, Rails uses a MVC (Model-View-Controller) architecture, which separates the data models, user interface, and business logic into distinct layers, making it easier to understand and update your application.

Finally, Ruby on Rails has an active and supportive community, with a wealth of resources, tutorials, and libraries available to help beginners learn and develop their application. This can be incredibly helpful when you're starting out, as you can learn from the experiences and expertise of others, and get help and advice when you need it.

To create a simple CRUD web application with Ruby on Rails, you'll need to have Ruby and Rails installed on your computer. If you don't already have them, you can follow the instructions on the [Ruby on Rails website](https://www.ruby-lang.org/en/documentation/installation/){:target="_blank"} to install them.

Once you have Ruby and Rails installed, you can create a new Rails project by running the following command in your terminal:

~~~sh
$ rails new travel-app
~~~

This will create a new Rails project called `travel-app` in a new directory with the same name.

Next, we'll need to create a database to store our data. In this example, we'll be using SQLite, which is a simple and easy-to-use database that comes pre-installed with Rails. To create the database, run the following command in your terminal:

~~~sh
$ rails db:create
~~~

With the database created, we can now create a model to represent the data we want to store. In this case, we'll create a model called `Destination` to represent a travel destination. To create the model, run the following command in your terminal:

~~~sh
$ rails generate model Destination name:string description:text
~~~

This will create a new model called `Destination` with two fields: `name` and `description`. The `name` field is a string, which means it can store text, and the `description` field is a text field, which means it can store longer pieces of text.

With the model created, we can now create a controller to handle the CRUD operations for our destinations. To create the controller, run the following command in your terminal:

~~~sh
$ rails generate controller destinations
~~~

This will create a new controller called `DestinationsController` with some basic methods for the CRUD operations.

Next, we'll need to define the routes for our controller. Routes are the URLs that users can visit in our application, and they map to the specific actions in our controller. To define the routes for our DestinationsController, open the `config/routes.rb` file and add the following code to it:

~~~ruby
Rails.application.routes.draw do
  resources :destinations
end
~~~

This code will configure the routes for all of the CRUD actions in our DestinationsController, using RESTful conventions.

With the routes and controller in place, we can now create the views for our CRUD operations. In Rails, views are HTML files that contain the layout and content for the web pages in our application. To create the views for our DestinationsController, we will edit the `.html.erb` files for each view.

First add the following content to the file `app/views/destinations/index.html.erb`:

~~~erb
<h1>Destinations</h1>

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Actions</th>
    </tr>
  </thead>
  <tbody>
    <% @destinations.each do |destination| %>
      <tr>
        <td><%= destination.name %></td>
        <td><%= destination.description %></td>
        <td>
          <%= link_to 'Show', destination %>
          <%= link_to 'Edit', edit_destination_path(destination) %>
          <%= link_to 'Destroy', destination, method: :delete, data: { confirm: 'Are you sure?' } %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Destination', new_destination_path %>
~~~

This html shows a table that displays all the destinations in the database, along with links to show, edit, and destroy each destination.

Next, open the `app/views/destinations/show.html.erb` file and add the following code to it:

~~~erb
<h1><%= @destination.name %></h1>

<p><%= @destination.description %></p>

<br>

<%= link_to 'Back', destinations_path %>
~~~

This code will display the name and description of the destination, along with a link to go back to the list of destinations.

Next, open the `app/views/destinations/new.html.erb` file and add the following code to it:

~~~erb
<h1>New Destination</h1>

<%= form_for @destination do |f| %>
  <div>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </div>

  <div>
    <%= f.label :description %>
    <%= f.text_area :description %>
  </div>

  <div>
    <%= f.submit %>
  </div>
<% end %>

<br>

<%= link_to 'Back', destinations_path %>
~~~

This code will create a form that allows the user to enter the name and description of a new destination.

Next, open the `app/views/destinations/edit.html.erb` file and add the following code to it:

~~~erb
<h1>Edit Destination</h1>

<%= form_for @destination do |f| %>
  <div>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </div>

  <div>
    <%= f.label :description %>
    <%= f.text_area :description %>
  </div>

  <div>
    <%= f.submit %>
  </div>
<% end %>

<br>

<%= link_to 'Back', destinations_path %>
~~~

This code shows the form that allows the user to edit the name and description of an existing destination.

With the views in place, we can now start our Rails server and visit the application in our web browser to test the CRUD operations. To start the server, run the following command in your terminal:

~~~sh
$ rails server
~~~

This will start the Rails server and make the application available at http://localhost:3000/.

To test the CRUD operations, visit the following URLs in your web browser:

- `http://localhost:3000/destinations`: This will display the list of destinations and allow you to create new destinations.
- `http://localhost:3000/destinations/1`: This will display the details of the destination with an ID of 1.
- `http://localhost:3000/destinations/1/edit`: This will allow you to edit the destination with an ID of 1.

That's it! You've successfully created a simple CRUD web application using Ruby on Rails and a travel domain as an example. There are many more features that you can add to this application, such as authentication, authorization, and validation, but this should give you a good starting point.

I hope this article has been helpful and has given you an idea of how to create a simple CRUD web application with Ruby on Rails. 

Happy coding! 

> Note:
> This is a simplified model of a travel app used just as an example. If you want a complete and functional ready to use travel app to plan your trips, you can check [Tripify Travel App](https://tripifyapp.com/){:target="_blank"}. 
