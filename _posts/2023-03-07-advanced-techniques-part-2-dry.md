---
title: "RSpec for Rails: Advanced Techniques #2 - DRY Testes"
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
date: 2023-03-07 08:20 +0100
last_modified_at: 2023-03-07 +0100
excerpt: "Discover the power of Shared Examples in RSpec for Rails and simplify your testing process with reusable and DRY code. Learn how to write shared examples for models, controllers, data validation, ensure associations, and define scopes all in one place. Streamline your testing process with Shared Examples in RSpec for Rails."
toc: true
toc_sticky: true
toc_label: "RSpec: DRY Tests"
tagline: "Don't Repeat Yourself: DRY Up Your RSpec Tests with Shared Examples for Your Travel App!"
header:
  teaser: assets/images/HelloMax-robot_testing_robot_b0542ad7-68b9-4f3c-9e28-c753f6fb7d5c.png
  overlay_image: assets/images/HelloMax-robot_testing_robot_b0542ad7-68b9-4f3c-9e28-c753f6fb7d5c.png
  overlay_filter: 0.65
---
## Shared Examples
Shared Examples are a powerful feature in RSpec that allows you to write reusable test scenarios that can be shared across multiple test files. By defining these shared examples, you can reduce the amount of duplicated code in your tests, making them easier to maintain and less prone to errors. This is particularly useful for large and complex applications like the Travel App that we are building in our article series.

In this article, we will explore how to use Shared Examples in RSpec to write more maintainable and efficient tests for the Travel App domain. We will cover DRY specs that make use of Shared Examples for both models and controllers, as well as other important aspects of the application. By the end of this article, you will have a good understanding of how to use Shared Examples to write efficient and maintainable tests for your own Rails applications.

## Models
When it comes to writing tests for your models, there are certain common behaviors that are often repeated across multiple models. For example, you might want to test the presence of validations, associations, and scopes on multiple models. In these cases, Shared Examples can be a very helpful tool.

o get started, we'll define our shared examples in a separate file, for example, `spec/support/shared_examples.rb`. In this file, we'll write tests that are common to multiple parts of our application. 

Here's an example of how to write a Shared Example for validations:
~~~ruby
RSpec.shared_examples 'validates presence' do |field|
  it "validates presence of #{field}" do
    subject.send("#{field}=", nil)
    expect(subject).to be_invalid
    expect(subject.errors[field]).to include("can't be blank")
  end
end
~~~

You can then include this shared example in each model's spec file, such as `spec/models/trips_spec.rb` and `spec/models/reviews_spec.rb`, by using the `it_behaves_like` method:
~~~ruby
# trips_spec.rb
describe Trip do
  it { is_expected.to have_many(:reviews) }
  it_behaves_like 'validates presence', :name
end

# reviews_spec.rb
describe Review do
  it { is_expected.to belong_to(:trip) }
  it_behaves_like 'validates presence', :content
end
~~~

Similarly, you can write shared examples for testing associations and scopes, which can then be included in each model's spec file. This helps you to write more DRY, maintainable, and readable tests.

## Controllers
When testing controllers in RSpec, it's common to write similar tests for different actions, such as index, create, update, and so on. Instead of repeating the same code in each controller's spec file, you can define these tests as shared examples and include them in each controller's spec file.

For example, let's say you have a TripsController and a ReviewsController in your Travel App. Both controllers have an index action that shows a list of trips or reviews, respectively. To test this action, you could write a shared example like this:
~~~ruby
shared_examples 'index action' do
  it 'renders the index template' do
    get :index
    expect(response).to render_template(:index)
  end

  it 'populates an array of records' do
    record1 = create(subject.controller_name.singularize)
    record2 = create(subject.controller_name.singularize)
    get :index
    expect(assigns(subject.controller_name)).to match_array([record1, record2])
  end
end
~~~

To include this shared example in the TripsController spec file, you would write:
~~~ruby
describe TripsController do
  it_behaves_like 'index action'
end
~~~

And in the ReviewsController spec file, you would write:
~~~ruby
describe ReviewsController do
  it_behaves_like 'index action'
end
~~~

With this shared example, you can now test the index action in both controllers with just one line of code in each spec file. This makes your tests DRY, reusable, and easier to maintain.

You can also write shared examples for other actions, such as create and update. For example:
~~~ruby
shared_examples 'create action' do
  it 'saves a new record in the database' do
    expect { post :create, params: { subject.controller_name.singularize => attributes_for(subject.controller_name.singularize) } }.to change(subject.controller_name.classify.constantize, :count).by(1)
  end

  it 'redirects to the show page' do
    post :create, params: { subject.controller_name.singularize => attributes_for(subject.controller_name.singularize) }
    expect(response).to redirect_to(assigns(subject.controller_name.singularize))
  end
end

shared_examples 'update action' do
  let!(:record) { create(subject.controller_name.singularize) }

  it 'updates the record in the database' do
    put :update, params: { id: record, subject.controller_name.singularize => { title: 'New title' } }
    record.reload
    expect(record.title).to eq('New title')
  end

  it 'redirects to the show page' do
    put :update, params: { id: record, subject.controller_name.singularize => { title: 'New title' } }
    expect(response).to redirect_to(record)
  end
end
~~~

And you would include these shared examples in each controller's spec file the same way as you did with the model shared examples. To use the shared examples in a controller spec file, simply include the line `it_behaves_like 'an action', action_name` in the appropriate context block. Where `action_name` is the name of the shared example you want to include.

By using shared examples for controllers, you can ensure that the same set of tests are run for each action, regardless of which controller it belongs to. This helps keep your tests DRY and organized, making them easier to maintain and debug.

## Views
It's also important to test the views of our application to make sure they are displaying the information correctly and in a way that is easy for users to understand. Like with models and controllers, we can also use shared examples in our view specs to avoid repeating the same tests in multiple spec files.

For example, let's say we have a `_form` partial in our Trip view that is used to display a form for creating or editing a Trip. We can create a shared example for this partial and include it in each of the Trip and Review views' spec files. This way, we only have to write the tests for the `_form` partial once, and we can reuse them for each view that uses this partial.

Here is an example of a shared example for the `_form` partial:
~~~ruby
shared_examples 'a form partial' do
  it 'displays the form' do
    render
    expect(rendered).to have_selector('form')
  end

  it 'displays the form fields' do
    render
    expect(rendered).to have_field('Name')
    expect(rendered).to have_field('Description')
  end

  it 'displays the submit button' do
    render
    expect(rendered).to have_button('Save')
  end
end
~~~

And you would include these shared examples in each view's spec file like this example:
~~~ruby
describe 'trip/new.html.erb' do
  it_behaves_like 'a form partial'
end
~~~

## Conclusion
In this article, we've explored the benefits of using shared examples in RSpec to write DRY, maintainable, and efficient tests for our Travel App. We covered how to write shared examples for Models, Controllers, and Views, and showed how to include these shared examples in each spec file.

By using shared examples, you can easily reuse common test logic across different parts of your application. This can help you write tests faster and reduce the amount of code you need to maintain, making it easier to catch bugs and keep your tests up-to-date.

We hope this article has inspired you to start using shared examples in your own projects. Whether you're a beginner or an experienced Rails developer, shared examples can help you write better tests and save time in the long run.

Don't forget to [subscribe to our newsletter](http://eepurl.com/igx0pj){:target='_blank'} to be the first to know about our next articles. Happy testing!
