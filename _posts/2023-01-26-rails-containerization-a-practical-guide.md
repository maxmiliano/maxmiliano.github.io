---
title: "Rails Containerization: A Practical Guide to Dockerizing Your App"
author: Max
tags:
  - Ruby
  - Rails
  - DevOps
  - Docker
  - programming
categories:
  - Tutorial
comments: true
related: true
date: 2023-01-26 08:47 -0300
last_modified_at: 2023-01-26 -0300
excerpt: "Dockerize your Rails application and elevate your development, testing, and deployment process with this step-by-step guide. Experience the reliability and efficiency of Docker for yourself and take your application to the next level."
toc: true
toc_sticky: true
toc_label: "Dockerizing an App"
tagline: "Unleash the power of Docker and take your Rails app to new heights with this easy-peasy step-by-step guide"
header:
  teaser: assets/images/whale_paiting_i_861ce3ca-1294-4e20-ba6a-8ae77145af65.png
  overlay_image: assets/images/whale_paiting_i_861ce3ca-1294-4e20-ba6a-8ae77145af65.png
  overlay_filter: 0.65
---
## Why Docker?
As a developer, I know that the process of deploying a Rails application can be a headache. But with Docker, you can ease the pain and take your application to the next level. In this guide, I'll walk you through the process of containerizing your Rails application, including a Postgres database, using Docker and Docker Compose.

From setting up the environment, to running and managing the containers, I'll guide you step-by-step. By the end of this guide, you'll have a running Rails application and a Postgres database, both running in separate containers managed by Docker Compose.

Whether you're a developer looking to improve your workflow, or a DevOps engineer looking to improve the scalability and portability of your application, Docker is something you'd like to add to your toolkit. Trust me, the benefits of using Docker are worth it, and I hope this guide will help you take the first step towards Dockerizing your Rails application.

## Prerequisites
* Docker 
  
  An instroduction to Docker and detailed steps to install it in you computer can be found on the the [official Docker site](https://www.docker.com/get-started/){:target='_blank'}.

* Docker Compose
  
  To install Docker Compose you can follow [these instructions](https://docs.docker.com/compose/install/){:target='_blank'}.

Please keep in mind that the installation process may vary depending on your operating system, so it's important to follow the instructions for your specific OS.

Also, you might want to check the official documentation of each tool for more information and for best practices.

## Dockerizing a Rails App

### Create a Docker File
    
Create a new file called `Dockerfile` in the root of your application. This file will contain the instructions for building the Docker image of your application.
~~~docker
FROM ruby:3.0

ENV RAILS_ROOT /app
RUN mkdir -p $RAILS_ROOT
WORKDIR $RAILS_ROOT

COPY . $RAILS_ROOT

RUN bundle install
~~~

This Dockerfile uses the `ruby:3.0` image as the base image, sets up an application environment for the app root, copies the application's code into the image, and then runs `bundle install` to install the application's dependencies.

### Create a Docker Compose file
Next, create a another file called `docker-compose.yml` also in the root of our application. This file will define the services (containers) in which the application will run.

~~~yaml
version: '3'
services:
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/app
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: postgres:10.7
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydb
~~~
In the `web` service definition:
* The `build` key specifies that the image should be built from the `Dockerfile` in the current directory. 
* The `command` key specifies the command to run when the container starts up, in this case starting the Rails server. 
* The `volumes` key is used to mount the application code into the container. 
* The `ports` key is used to map the container's port 3000 to port 3000 on the host.
* The `depends_on` key specifies that the `web` service depends on the `db` service, so the `db` service will be started first, no matter the order they are in our file.

In the `db` service definition:
* The `image` key specifies the image to use, in this case `postgres:10.7`. 
* The `environment` key is used to set environment variables for the service.

### Building and running
Now you can use the `docker-compose up` command to build and run the application. The first time you run this command, it will take a while as it will have to download the necessary images and build the application.

This command will start the web and db services as defined in our `docker-compose.yml` file. It will also build the web service image if it does not exist.

You can see the logs of the application by running `docker-compose logs -f` command.

Once the application is running you can access it by visiting `http://localhost:3000` in your browser.

You can stop the application by running `docker-compose down` command.

You can also use other commands like `docker-compose start`, `docker-compose stop`, `docker-compose restart` to **start**, **stop**, and **restart** the application respectively.

Please keep in mind that this is a general overview of the process and that depending on your application's specific requirements, you may need to make additional modifications to the `Dockerfile`, the `docker-compose.yml` file, or your application code to get everything working as expected.

You may also need to include additional services in your `docker-compose.yml` file, such as `redis`, `sidekiq`, or `nginx`, depending on the requirements of your application. However, we can discuss these in more detail in a separate article.

## Conclusion

Dockerizing your Rails application offers a wealth of advantages. By using Docker, you can ensure consistency across all environments, making testing and deployment a breeze. The ability to package your application and its dependencies into a single container makes distribution and running the application a hassle-free experience. And with Docker Compose, managing and running multiple containers for your application, including a database, is a seamless process. Not to mention, containerizing your application allows for improved scalability and portability.

Docker is a valuable tool that can elevate the development, testing, and deployment of your application, making it more reliable and efficient. Following the steps outlined in this guide, you can containerize your Rails application and start experiencing the benefits of Docker for yourself.

It's worth noting that Docker is not just a tool for production deployment, it's also a great tool for development environments, you can use it to ensure that your developers are working on the same environment, and you can avoid those frustrating "It works on my machine" situations.

In this guide, I've shown you how to Dockerize your Rails application and manage it with Docker Compose. I hope you find this guide helpful and that it will allow you to take your application to the next level.

Happy coding (and deploying)!
