---
title: "Adding another model (and more associations) to our RoR app"
author: Max
tags:
  - Ruby
  - Rails
  - code
  - programming
comments: true
related: true
date: 2023-01-19 08:47 -0300
last_modified_at: 2023-01-19 08:47 -0300
excerpt: "In this tutorial, we will be adding a Trip model to a Rails app. Each Trip can have one or more Destinations and is created by a User. A Trip also has a start date and end date."
toc: true
toc_sticky: true
toc_label: "Adding another model"
header:
  teaser: assets/images/train-transportation-winter-season-3758523-3758523_1920.jpg
  overlay_image: assets/images/train-transportation-winter-season-3758523-3758523_1920.jpg
  overlay_filter: 0.65
  caption: Photo from [Pixabay](https://pixabay.com/photos/train-transportation-winter-season-3758523/)
---

## What we will do
In this tutorial, we will be adding a Trip model to a Rails app. Each Trip can have one or more Destinations and is created by a User. A Trip also has a start date and end date.

To fullfill this requirements in addition to creating a new Trip model we will also need to do a few changes on existing models. And we will learn a new type of relationship as well.

## Step 1: Generate the Trip model and migration

Run the following command to generate the Trip model and migration:
~~~sh
rails generate model Trip start_date:date end_date:date user:references
~~~
This will create a Trip model and a migration file to add the necessary database columns for the Trip model.

## Step 2: Set up the Destination-Trip association

In the `Trip` model, set up a `has_and_belongs_to_many` association with the `Destination` model. This will allow a `Trip` to have many Destinations, and a `Destination` to be part of many Trips.

~~~ruby
# app/models/trip.rb

class Trip < ApplicationRecord
  has_and_belongs_to_many :destinations
  belongs_to :user
end
~~~
In the `Destination` model, set up a `has_and_belongs_to_many` association with the `Trip` model.

~~~ruby
# app/models/destination.rb

class Destination < ApplicationRecord
  has_and_belongs_to_many :trips
  has_many :reviews
end
~~~

## Step 3: Create the join table
Run the following command to create the necessary database table for the `has_and_belongs_to_many` association between `Trips` and `Destinations`:

~~~sh
rails generate migration CreateJoinTableTripDestination trip destination
~~~
This will create a migration file with the necessary code to create the join table for the `has_and_belongs_to_many` association between `Trips` and `Destinations`. If you are faimiliar with Relational Database, this is an Associative Table that solves an `n-n` relationship. But the magic of RoR's Active Record will handle this for us.

## Step 4: Run the migration
Run the migration to create the necessary database columns and tables for the Trip model.

~~~sh
rails db:migrate
~~~

## Step 5: Add validations to the Trip model
Add validations to the Trip model to ensure that a Trip has a start date, end date, and user.
Additionally, add a validation to ensure that the end date is the same as or after the start date.
~~~ruby
# app/models/trip.rb

class Trip < ApplicationRecord
  has_and_belongs_to_many :destinations
  belongs_to :user

  validates :start_date, presence: true
  validates :end_date, presence: true
  validate :end_date_after_start_date
  validates :user, presence: true

  private

  def end_date_after_start_date
    if end_date && start_date && end_date < start_date
      errors.add(:end_date, "must be the same as or after the start date") 
    end
  end
end
~~~

## Step 6: Add a form for creating a new Trip
Add a form to a view file for creating a new `Trip`. The form should include fields for the `start date`, `end date`, and `Destinations`. As a `Trip` may have one or manh Destinations, you can use the `collection_check_boxes` helper to create checkboxes for selecting one or more `Destinations`.

Your view file at `app/views/trips/new.html.erb` should look similar to this:
~~~erb
<%= form_for @trip do |f| %>
  <%= f.label :start_date %>
  <%= f.date_field :start_date %>

  <%= f.label :end_date %>
  <%= f.date_field :end_date %>

  <%= f.label :destinations %>
  <%= f.collection_check_boxes :destination_ids, Destination.all, :id, :name %>

  <%= f.submit %>
<% end %>
~~~
This code will create a form with fields for the Trip. The `collection_check_boxes` is a Rails helpe that will create a checkbox for each Destination in the database, allowing the user to select one or more Destinations for the Trip. When we have a lot of Destinations in our database, we may want to change this. But for now this will work fine on our app.

## Step 7: Create a new Trip in the controller
In the controller action that handles the form submission, create a new Trip using the form params. Set the user attribute of the Trip to the current user.

~~~ruby
# app/controllers/trips_controller.rb

class TripsController < ApplicationController
  def create
    @trip = Trip.new(trip_params)
    @trip.user = current_user

    if @trip.save
      redirect_to @trip
    else
      render :new
    end
  end

  private

  def trip_params
    params.require(:trip).permit(:start_date, :end_date, destination_ids: [])
  end
end
~~~
> Note: Keep in mind that your `TripsController` will need more actions to deal with the needed logic. If you need more context on how to that you can check our previous articles: [*Get started with Ruby on Rails*]({% post_url 2022-12-20-get-started-with-ruby-on-rails %}) and [*Expand Your RoR Travel*]({% post_url 2022-12-27-expand-your-travel-crud-app %}).

## Step 8: Display the Trip details
In the view for displaying a single Trip, show the start date, end date, and Destinations for the Trip.

You can add the following code to the `app/views/trips/show.html.erb` file:
~~~erb
<p>Start date: <%= @trip.start_date %></p>
<p>End date: <%= @trip.end_date %></p>
<p>Destinations:</p>
<ul>
  <% @trip.destinations.each do |destination| %>
    <li><%= destination.name %></li>
  <% end %>
</ul>
~~~
This code will display the start date, end date, and a list of Destinations for the Trip. The `@trip.destinations` method returns an array of `Destination` objects associated with the `Trip`, and the each loop iterates over the array, displaying the name of each `Destination`.

You can also include links for editing and deleting the `Trip`, as well as a list of `Reviews` for each `Destination`.
~~~erb
<p>Start date: <%= @trip.start_date %></p>
<p>End date: <%= @trip.end_date %></p>
<p>Destinations:</p>
<ul>
  <% @trip.destinations.each do |destination| %>
    <li>
      <%= destination.name %>
      <%= link_to "Edit Destination", edit_destination_path(destination) %>
      <%= link_to "Delete Destination", destination, method: :delete, data: { confirm: "Are you sure?" } %>
      <% if destination.reviews.any? %>
        <ul>
          <% destination.reviews.each do |review| %>
            <li>
              <%= review.rating %>
              <%= review.comment %>
              <%= link_to "Edit Review", edit_review_path(review) %>
              <%= link_to "Delete Review", review, method: :delete, data: { confirm: "Are you sure?" } %>
            </li>
          <% end %>
        </ul>
      <% end %>
    </li>
  <% end %>
</ul>

<%= link_to "Edit Trip", edit_trip_path(@trip) %>
<%= link_to "Delete Trip", @trip, method: :delete, data: { confirm: "Are you sure?" } %>
~~~
This code will display the start date, end date, and a list of Destinations for the Trip. For each Destination, it will display a list of Reviews with ratings and comments. It will also include links for editing and deleting each Destination and Review, as well as a link for deleting the entire Trip.

## Conclusion
In this tutorial, we learned how to add a `Trip` model to a Rails app. We generated the `Trip` model and migration, set up the `Destination-Trip` association, created the join table, ran the migration, added validations to the `Trip` model, added a form for creating a new `Trip`, created a new `Trip` in the controller, and displayed the `Trip` details in the view along with its attributes.

I hope this tutorial was helpful. If you have any questions or comments, please don't hesitate to reach out.

Happy coding!
