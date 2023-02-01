---
title: "RSpec for Rails: Testing with Efficient Specs"
author: Max
tags:
  - Ruby
  - Rails
  - Tests
  - RSpec
  - programming
categories:
  - Tutorial
comments: true
related: true
date: 2023-02-21 08:32 -0300
last_modified_at: 2023-02-21 -0300
excerpt: "In this article, we delve into the world of RSpec testing in a Ruby on Rails application. By leveraging the power of RSpec, we can ensure that our application is functioning as expected, catch bugs early in development, and save time by preventing regressions in the future. We'll show you how to write efficient tests for your Rails models and controllers, using examples from a sample app featuring Trip and Destination models. We'll also explore how to use Devise helpers for testing the User model, and provide code examples for each scenario. Whether you're a seasoned Rails developer or just starting out, this article will give you a solid foundation for RSpec testing in your next project."
toc: true
toc_sticky: true
toc_label: "Writing specs"
tagline: "Specs, Not Just For Glasses: The Ultimate Guide to Testing Your Rails App"
header:
  teaser: assets/images/HelloMax_RSpec_tests_682e62dd-c43d-4b86-a358-ca1483319a1e.png
  overlay_image: assets/images/HelloMax_RSpec_tests_682e62dd-c43d-4b86-a358-ca1483319a1e.png
  overlay_filter: 0.65
---
## Why testing?
Testing is an essential part of the development process, and RSpec is one of the most popular testing frameworks for Rails. In this article, we will go over the basics of using RSpec for testing in a Rails app, and provide examples of how to write efficient and effective tests for the Trip, Destination, and User models, as well as the TripsController.

## Setting up RSpec in Rails App
In [my previous article]({% post_url 2023-02-14-rspec-for-rails-adding-the-gem %} "RSpec for Rails: Adding the gem and starting testing") I showed how to add RSpec to an existing Rails App and how to start writing tests. Here's a quick catch up on how to do that.
{: .notice--info}

To get started with RSpec in a Rails app, you'll need to add the RSpec gem to your Gemfile and run the `bundle install` command. You'll also need to run the `rails generate rspec:install` command to set up the necessary configuration files.

Once you've done that, you can start writing tests by creating a new file in the `spec` directory with a `_spec.rb` extension. For example, to write tests for the Trip model, you would create a new file called `trip_spec.rb` in the `spec/models` directory.

## Writing Model Specs
When writing model specs, it's important to focus on testing the validations, associations, and methods of the model. For example, in the case of the **Trip** model, you might write tests to ensure that a trip has a `start_date` and `end_date`, and that the `user` association is set up correctly.

Here's an example of how you might write specs for the **Trip** model:
~~~ruby
require 'rails_helper'

RSpec.describe Trip, type: :model do
  subject(:trip) { create(:trip, user: user) }
  let(:user) { create(:user) }

  it { should validate_presence_of(:start_date) }
  it { should validate_presence_of(:end_date) }
  it { should belong_to(:user) }
  it { should have_and_belong_to_many(:destinations) }

  describe '#duration' do
    it 'calculates the duration of the trip' do
      expect(trip.duration).to eq(trip.end_date - trip.start_date)
    end
  end
end
~~~
In this RSpec test, we are testing the **Trip** model. The first part of the test sets up two variables: `trip` and `user`. The `user` variable is created using the `create` method and the `:user` factory. The `trip` variable is created using the `create` method and the `:trip` factory, and it is passed the `user: user` argument to associate the trip with the user. 

The `create` method is provided by the `factory_bot` gem, which helps us on creating instances for our tests.
{: .notice--info}

Next, we have four `it` blocks that use the `should` syntax to test for various validations and associations on the **Trip** model. The first two blocks use the `validate_presence_of` method to test that the `start_date` and `end_date` fields are required. The third block uses the `belong_to` method to test that the **Trip** model belongs to the **User** model. The fourth block uses the `have_and_belong_to_many` method to test that the **Trip** model has a many-to-many relationship with the **Destination** model.

Finally, we have a `describe` block that tests the `duration` method on the **Trip** model. The `it` block inside the `describe` block uses the `expect` method to test that the result of the `duration` method is equal to the difference between the `end_date` and `start_date` fields of the trip.

You may also want to test the Destination model, and to do so, you can use the same approach.

## Writing Controller Specs
When writing controller specs, it's important to focus on testing the behavior of the controller's actions. For example, in the case of the **TripsController**, you might write tests to ensure that the `index` action returns a list of trips, and that the `create` action creates a new trip and redirects to the trip's show page.

Here's an example of how you might write specs for the **TripsController**:
~~~ruby
require 'rails_helper'

RSpec.describe TripsController do
  let(:user) { create(:user) }
  let(:destination) { create(:destination) }
  let(:trip) { create(:trip, user: user, destinations: [destination]) }

  describe "GET #index" do
    it "returns a success response" do
      get :index
      expect(response).to be_successful
    end

    it "assigns all trips as @trips" do
      get :index
      expect(assigns(:trips)).to eq([trip])
    end
  end

  describe "GET #show" do
    it "returns a success response" do
      get :show, params: {id: trip.to_param}
      expect(response).to be_successful
    end

    it "assigns the requested trip as @trip" do
      get :show, params: {id: trip.to_param}
      expect(assigns(:trip)).to eq(trip)
    end
  end

  describe "GET #new" do
    it "returns a success response when user is logged in" do
      sign_in user
      get :new
      expect(response).to be_successful
    end

    it "redirects to sign in page when user is not logged in" do
      get :new
      expect(response).to redirect_to(new_user_session_path)
    end
  end

  describe "GET #edit" do
    it "returns a success response when user is logged in and is the trip's owner" do
      sign_in user
      get :edit, params: {id: trip.to_param}
      expect(response).to be_successful
    end

    it "redirects to trip's show page when user is not the trip's owner" do
      other_user = create(:user)
      sign_in other_user
      get :edit, params: {id: trip.to_param}
      expect(response).to redirect_to(trip_path(trip))
    end

    it "redirects to sign in page when user is not logged in" do
      get :edit, params: {id: trip.to_param}
      expect(response).to redirect_to(new_user_session_path)
    end
  end

  describe "POST #create" do
    context "with valid params" do
      it "creates a new Trip" do
        sign_in user
        expect {
          post :create, params: {trip: trip.attributes}
        }.to change(Trip, :count).by(1)
      end

      it "redirects to the created trip" do
        sign_in user
        post :create, params: {trip: trip.attributes}
        expect(response).to redirect_to(Trip.last)
      end
    end

    context "with invalid params" do
      it "does not create a new trip and returns unprocessable entity status code" do
        sign_in user
        expect { post :create, params: {trip: {}} }.to_not change(Trip, :count)
        expect(response).to have_http_status(:unprocessable_entity)
      end
    end
  end
end
~~~
In the example above, the first line creates a describe block for the **TripsController**. Within the describe block, you'll write tests to check the behavior of the controller.

Next, we use the `context` method to specify different scenarios that we want to test. 

For example, in the first context block, we test the behavior of the `index` action. Here, we create a `before` block to set up any data that is needed for the test. In this case, we create an instance variable `@trips` and assign it to an array of trips, which will be used in the controller's index action.

After that, we use the `it` method to write a test that specifies what we expect to happen when the controller's index action is executed. In this case, we expect the `@trips` instance variable to be assigned and for the `index` template to be rendered.

In the last `context` block, we test the behavior of the `create` action. This time, we use the `post` method to simulate a POST request to the `create` action and pass along the necessary parameters. We then use the `expect` method to test the response and check if the `create` action redirecteds to the trip's `show` page. Note that we have a `context` for both `with valid params` and `with invalid params` cases and we have `it` blocks that specifies what to expect in each scenario.

While we didn't cover every block of the **TripsController** spec, the examples provided give a good idea of how to write tests for your controllers in Rails. This knowledge should also make you able to write the specs for the **DestinationsController** and even for your on controllers in any of your Rails apps.  

## Conclusion
It's important to test all aspects of your application to make sure it works as expected. By using RSpec to write efficient and comprehensive tests, you'll be able to catch bugs and edge cases early on, which will save you time and headaches in the long run.

If you want to read more about tests in Ruby on Rails or any other aspect of software devlopment, career and tech news, be sure to [subscribe to our blog newsletter](http://eepurl.com/igx0pj){:target="_blank"} to stay updated. And if you have any question or suggestion about this article, please leve a comment.
{: .notice--info}

**Happy testing!**
