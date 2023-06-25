---
title: "Simplifying Authentication with HTTP Basic Auth in Rails"
#subtitle: ""
author: Max
tags:
  - Ruby
  - Rails
  - code
  - identity
categories:
  - Tutorial
comments: true
related: true
date: 2023-06-26 07:32 +0100
last_modified_at: 2023-06-26 07:32 +0100
excerpt: "Dive into the world of Rails and learn a simplified, no-nonsense way to authenticate your app. HTTP Basic Authentication might not be the flashiest bouncer at the door, but it sure gets the job done when you need a quick and easy solution!"
toc: true
toc_sticky: true
tagline: "HTTP Basic Authentication: It's like the fast food of the authentication world - not fancy, but quick, reliable and hits the spot when you're in a hurry!"
header:
  teaser: assets/images/HelloMax_basic_http_authentication-key_and_vault-cc362547-18ab-4321-b4dc-4d0c9851c8a5.png
  overlay_image: assets/images/HelloMax_basic_http_authentication-key_and_vault-cc362547-18ab-4321-b4dc-4d0c9851c8a5.png
  overlay_filter: 0.65
---
Hello again, fellow coders!

In a previous article, we delved deep into [adding the Devise gem to a Rails app for authentication]({% post_url 2023-01-10-adding-authentication-to-a-rails-app-with-devise  %}). Devise is powerful and full of features, but it might be too much overpower for some use cases. Today, we'll explore an alternative - HTTP Basic Authentication. I recently had the opportunity to implement this in [SeQura](https://www.sequra.com/){:target="_blank"}, and I'm excited to share this experience with you.

## When to use HTTP Basic Auth?

Basic HTTP authentication can serve as a more straightforward alternative to more comprehensive solutions like Devise. But when exactly should we consider Basic Auth?

1. **Development Phase**: If your application is still in the early stages of development and you need a quick way to test authentication, Basic Auth could be an ideal fit.

1. **Single User Administrative Panel**: If your app requires just one administrative user without the need for complex permission sets, Basic Auth will suffice.

1. **External User**: If you need to allow a user from outside your project / company that has no credentials such as SSO or a specific email to access your app for whatever reason (audition, validation or anything similar), Basic Auth may be a good option.

It's not recommended for scenarios where you need to handle multiple users, complex permission sets, and high-security standards, but for simpler use-cases, it hits the mark.

## Implementing HTTP Basic Auth in Rails
Before we start coding, let's understand how we'll handle the username and password. For simplicity's sake, we'll store them in a secrets.yml file in plain text. However, this is merely for example purposes. In a real-world application, you should always encrypt and store your passwords securely.

Here's how our `secrets.yml` file looks:

~~~yaml
development:
  admin_name: 'admin'
  admin_password: 'password'
~~~

Next, we'll include a `before_action` in the ApplicationController to enforce the authentication:

~~~ruby
class ApplicationController < ActionController::Base
  before_action :http_authenticate

  def http_authenticate
    return if Rails.env.development?
    
    authenticate_or_request_with_http_basic do |username, password|
      username == Rails.application.secrets.admin_name && password == Rails.application.secrets.admin_password
    end
  end
end
~~~

This will require HTTP Basic Authentication for all controllers inheriting from the ApplicationController in the development environment.

The `authenticate_or_request_with_http_basic` method is a Rails helper for implementing **HTTP Basic Authentication**. It first checks if the HTTP Authorization header is present and correctly formatted in the request. The header should contain the credentials in a specific format, which browsers automatically add when they detect a request for HTTP Basic Auth.

If the Authorization header is present and correct, the method decodes the credentials (username and password) and passes them to a provided block. This block contains your logic to validate the credentials, typically by comparing them against the expected username and password.

If the credentials are valid (i.e., the block returns true), the request is authenticated and processing continues. If the credentials are not valid (i.e., the block returns false), or if the Authorization header is missing or incorrect, the method sends an 401 Unauthorized HTTP response to the client. This response includes a WWW-Authenticate header, which prompts the browser to display a login dialog to the user. So, in essence, `authenticate_or_request_with_http_basic` handles the entire process of checking and validating Basic Auth credentials and prompting the user to enter their credentials if necessary.

What if you need to authenticate only specific controllers? It's simple. Just move the `before_action` from the ApplicationController to the specific controller where you want the Basic Auth.

For example:

~~~ruby
class AdminController < ApplicationController
  before_action :http_authenticate
  ...
end
~~~

The `AdminController` and any other Controller that inherits from it will require Basic Authentication. But any other one will keep working the same way before.

And there you have it - a simple, straightforward way to implement HTTP Basic Auth in your Rails app!

## Wrapping Up
While Devise and similar gems offer robust solutions for more complex needs, HTTP Basic Authentication is a handy tool for simpler scenarios. Its quick implementation time and ease of use make it an excellent choice for certain use-cases.

Before we go, let's remind ourselves - keeping passwords secure is of utmost importance. Always ensure that you're encrypting passwords when working on production applications.

Want more Rails tips and insights like this? [Subscribe to my newsletter for regular updates.](http://eepurl.com/igx0pj) 

Let's continue to learn and grow our skills together!

Happy coding!
