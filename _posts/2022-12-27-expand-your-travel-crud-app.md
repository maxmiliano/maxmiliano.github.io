---
title: Expand Your RoR Travel CRUD App adding associations
author: Max
tags:
  - Ruby
  - Rails
  - code
  - CRUD
  - programming
comments: true
related: true
date: 2022-12-27 09:12 -0300
header:
  teaser: assets/images/ed-259-xcrI6CPkkJs-unsplash.jpg
  overlay_image: assets/images/ed-259-xcrI6CPkkJs-unsplash.jpg
  overlay_filter: 0.65
---

In the [previous article]({% post_url 2022-12-20-get-started-with-ruby-on-rails %}), we showed you how to create a simple CRUD web application for a travel domain using Ruby on Rails. In this article, we'll take that application a step further and add one more model and associations to it to make it more robust.

One of the key features of Ruby on Rails is its ability to easily associate models and create relationships between them. This allows you to build more complex and dynamic applications, where the data is connected in meaningful ways.

For example, in our travel CRUD application, we might want to add a model for `Reviews` to allow users to write reviews of the destinations they've visited. We can create this model by running the following command in our terminal:

~~~sh
$ rails generate model Review rating:integer comment:text destination:references
~~~

This will create a new model called `Review` with three fields: `rating` (an integer), `comment` (a text field), and `destination` (a reference to the Destination model).

To create the association between the Review and Destination models, we'll need to add a `has_many` and `belongs_to` relationship to our models. Open the `app/models/destination.rb` file and add the following code to it:

~~~ruby
class Destination < ApplicationRecord
  has_many :reviews
end
~~~

This code tells Rails that a Destination can have many Reviews.

Next, open the `app/models/review.rb` file and add the following code to it:

~~~ruby
class Review < ApplicationRecord
  belongs_to :destination
end
~~~

This code tells Rails that a Review belongs to a Destination.

With the models and associations in place, we can now update our controllers and views to allow users to create, read, update, and delete reviews for each destination.

To do this, we'll need to update the DestinationsController to show the reviews for each destination and allow users to create new reviews. Open the `app/controllers/destinations_controller.rb` file and add the following code to the `show` action:

~~~ruby
def show
  @destination = Destination.find(params[:id])
  @review = Review.new
  @reviews = @destination.reviews
end
~~~

This code will find the destination with the ID specified in the URL, create a new review for that destination, and fetch all of the reviews for that destination.

Next, we'll need to update the `create` action in the ReviewsController to create a new review for a destination. Open the `app/controllers/reviews_controller.rb` file and add the following code to the `create` action:

~~~ruby
def create
  @review = Review.new(review_params)
  @review.destination_id = params[:destination_id]

  if @review.save
    redirect_to destination_path(params[:destination_id])
  else
    render 'new'
  end
end
~~~

This code will create a new review with the parameters specified in the form, set the destination ID for the review, and save it to the database. If the review is saved successfully, the user will be redirected to the destination page, otherwise, the `new` view will be rendered.

Next, we'll need to update the `show.html.erb` view for the Destination model to display the reviews and allow users to create new reviews. Open the `app/views/destinations/show.html.erb` file and add the following code to it:

~~~erb
<h1><%= @destination.name %></h1>

<p><%= @destination.description %></p>

<h2>Reviews</h2>

<%= render 'reviews/form', review: @review %>

<table>
  <thead>
    <tr>
      <th>Rating</th>
      <th>Comment</th>
      <th>Actions</th>
    </tr>
  </thead>
  <tbody>
    <% @reviews.each do |review| %>
      <tr>
        <td><%= review.rating %></td>
        <td><%= review.comment %></td>
        <td>
          <%= link_to 'Edit', edit_review_path(review) %>
          <%= link_to 'Destroy', review, method: :delete, data: { confirm: 'Are you sure?' } %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'Back', destinations_path %>
~~~

This code will display the reviews for the destination, along with links to edit and destroy each review. It will also render the form for creating a new review.

Next, we'll need to create the form partial for creating new reviews. To do this, create a new file called `_form.html.erb` in the `app/views/reviews` directory and add the following code to it:

~~~erb
<%= form_for([@destination, @review]) do |f| %>
  <div>
    <%= f.label :rating %>
    <%= f.number_field :rating %>
  </div>

  <div>
    <%= f.label :comment %>
    <%= f.text_area :comment %>
  </div>

  <div>
    <%= f.submit %>
  </div>
<% end %>
~~~

This code will create a form that allows the user to enter the rating and comment for a new review.

With the views in place, we can now start our Rails server and visit the application in our web browser to test the review CRUD operations. To start the server, run the following command in your terminal:

~~~sh
$ rails server
~~~

This will start the Rails server and make the application available at `http://localhost:3000/`.

To test the review CRUD operations, visit the following URLs in your web browser:

- `http://localhost:3000/destinations/1`: This will display the details of the destination with an ID of 1, along with a form for creating a new review and a list of reviews for that destination.
- `http://localhost:3000/reviews/1/edit`: This will allow you to edit the review with an ID of 1.

That's it! You've successfully expanded your travel CRUD application to include more models and associations, and you've learned how to use the `has_many` and `belongs_to` relationships to connect those models in meaningful ways. From now on you may expand the domain adding more models and building relationship between them.

There are many more features that you can add to this application, such as authentication, authorization, and validation, but this should give you a good starting point.

I hope this article has been helpful and has given you an idea of how to create more complex and dynamic web applications using Ruby on Rails. 

Happy coding!

> Note:
> This is a simplified model of a travel app used just as an example. If you want a complete and functional ready to use travel app to plan your trips, you can check [Tripify Travel App](https://tripifyapp.com/){:target="_blank"}. 

