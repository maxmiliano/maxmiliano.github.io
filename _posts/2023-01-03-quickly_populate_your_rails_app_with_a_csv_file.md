---
title: Quickly Populate Your Rails App with a CSV File
author: Max
tags:
  - Ruby
  - computer science
  - code
  - database
  - programming
comments: true
related: true
date: 2023-01-03 09:18 -0300
header:
  teaser: assets/images/pomegranate-g7b474234c_1920.jpg
---

In this article we will learn you how to seed data to a Rails application from a CSV file. For this example we will use the Destination model from the travel theme app we built in our past two articles as an example, but this applies to any Ruby on Rails app that has Active Record models. The Destination model has the following attributes: name (string) and description (text).

We will be using a CSV file to seed the Destination model with data. The CSV file will contain the names and descriptions of ten different destinations.

## Step 1: Create the CSV file
Create a CSV file with the data that you want to seed. Make sure that the columns in the CSV file match the attributes of the models in your Rails app. For example, if you want to seed the Destination model with names and descriptions, your CSV file might look something like this:

~~~text
name,description
Barcelona,"A city in Spain known for its beaches, architecture, and lively atmosphere."
Rome,"The capital of Italy and a city with a rich history and cultural landmarks."
Paris,"The capital of France and a city known for its art, fashion, and cuisine."
Sydney,"The largest city in Australia and a popular destination known for its iconic Opera House and beautiful beaches."
New York City,"The most populous city in the United States and a global hub for finance, entertainment, and culture."
San Francisco,"A city in California known for its hilly terrain, cultural diversity, and iconic Golden Gate Bridge."
Toronto,"The largest city in Canada and a diverse metropolis known for its multicultural neighborhoods and CN Tower."
Buenos Aires,"The capital and largest city of Argentina, known for its tango music, soccer, and cultural landmarks."
Rio de Janeiro,"A city in Brazil known for its beautiful beaches, lively atmosphere, and iconic Christ the Redeemer statue."
Cape Town,"A city in South Africa known for its stunning natural beauty, including Table Mountain and the Cape of Good Hope."
~~~

## Step 2: Create the seeds.rb file
In your Rails app, create a new file in the `db/seeds` directory called `seeds.rb`. This file will contain the code that will be used to seed your database.

## Step 3: Require the CSV library and open the CSV file
In the `seeds.rb` file, require the CSV library and open the CSV file using the `CSV.foreach` method. This method allows you to iterate over the rows in the CSV file and perform an action for each row.

~~~ruby
require 'csv'

csv_text = File.read(Rails.root.join('db', 'seeds', 'destinations.csv'))
csv = CSV.parse(csv_text, :headers => true, :encoding => 'ISO-8859-1')
~~~

## Step 4: Create a new instance of the model and save it to the database
Within the block passed to the `CSV.foreach` method, create a new instance of the Destination model and set the attributes using the values from the current row in the CSV file. Then, use the save method to save the instance to the database.

~~~ruby
csv.each do |row|
  destination = Destination.new
  destination.name = row['name']
  destination.description = row['description']
  destination.save
end
~~~

## Step 5: Run the seed data task
Run the seed data task using the following command: rails db:seed. This will execute the code in the seeds.rb file and seed the database with the data from the CSV file.

~~~sh
$ rails db:seed
~~~

That's it! We have now successfully seeded our Rails app with data from a CSV file. You can repeat these steps for any other model that you want to seed, such as the Review model in our travel app, with data from a CSV file. Just make sure that the columns in the CSV file match the attributes of the model, and that you are using the correct model.

Hope you enjoyed this article and please leave a comment if you have any question or suggestion.

Hapy coding!

