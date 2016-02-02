---
layout: post
title:  "How to deploy a Ruby on Rails app to Heroku"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: heroku
credits:
 - https://github.com/heroku/heroku
 - https://devcenter.heroku.com/categories/command-line
 - https://devcenter.heroku.com/articles/ruby-versions
 - https://devcenter.heroku.com/articles/ruby-default-web-server
 - https://devcenter.heroku.com/articles/getting-started-with-rails4#done
 - https://devcenter.heroku.com/articles/heroku-postgresql#connection-permissions
 - https://coderwall.com/p/6bmygq/heroku-rails-bower
 - https://github.com/heroku/heroku-buildpack-nodejs
 - https://github.com/heroku/heroku-buildpack-ruby
 - http://www.nateberkopec.com/2015/07/29/scaling-ruby-apps-to-1000-rpm.html
---

This document describes the process of deploying a Ruby on Rails application to a production server hosted by Heroku.

## Prerequisites

Download [heroku toolbelt](https://toolbelt.heroku.com/) to enable `heroku` command line tools.

```` sh
heroku login
cd ~/myapp
````

## Configuration

Create and configure a new heroku application.

````
heroku create
heroku apps:rename myapp
heroku config:set KEY1=VALUE1 [KEY2=VALUE2 ...]
# heroku domains:add example.com
````

### Custom Buildpacks

If your rails app uses [node package manager](https://www.npmjs.com/) to manage front-end dependencies, configure heroku to use a custom [buildpack](https://devcenter.heroku.com/articles/buildpacks) called [buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi), which facilitates usage of multiple heroku [buildpacks](https://elements.heroku.com/buildpacks).

````sh
heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git
````

Specify buildpacks in a **.buildpacks** file:

    https://github.com/heroku/heroku-buildpack-nodejs
    https://github.com/heroku/heroku-buildpack-ruby

> NOTE: the practice of setting BUILDPACK_URL is [deprecated](https://devcenter.heroku.com/articles/buildpacks#using-a-custom-buildpack). The preferred method is `heroku buildpacks:set https://github.com/ddollar/heroku-buildpack-multi.git`. todo: test this out.

## Deployment

Deploy:

````sh
git push heroku master
heroku open
````

## Suppress Warnings

Add to Gemfile:

````
ruby "2.2.0"
group :production do
  gem 'rails_12factor'
  gem 'puma'
end
````

Add a Procfile:

````
web: bundle exec puma -C config/puma.rb
````

Add a config/puma.rb:

```` rb
workers Integer(ENV['WEB_CONCURRENCY'] || 2)
threads_count = Integer(ENV['MAX_THREADS'] || 5)
threads threads_count, threads_count

preload_app!

rackup      DefaultRackup
port        ENV['PORT']     || 3000
environment ENV['RACK_ENV'] || 'development'

on_worker_boot do
  # Worker specific setup for Rails 4.1+
  # See: https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server#on-worker-boot
  ActiveRecord::Base.establish_connection
end

````

Also make sure there is a root route set in config/routes.rb.

## Add-ons

```` sh
heroku addons
````

### Database

Follow the PostgreSQL or MySQL instructions, depending on your database choice.

For further instructions, see [Working with Databases on Heroku](http://data-creative.info/process-documentation/2015/08/17/databases-on-heroku.html).

#### PostgreSQL

```` sh
heroku pg:credentials DATABASE
heroku pg:reset DATABASE
heroku run bundle exec rake db:migrate
heroku run bundle exec rake db:seed
````

> `heroku pg:reset DATABASE` is roughly equivalent to `rake db:drop && rake db:create`

Use `heroku pg:psql` to execute custom SQL queries in a production database console.

#### MySQL

Revise `database.yml`:

```` yaml
production:
  url: <%= ENV['DATABASE_URL'] %>
````

Create a new `DATABASE_URL` heroku config variable as a duplicate of the `CLEARDB_DATABASE_URL`
 except change the `mysql` part to `mysql2`. This avoids a [mysql gem installation error](http://stackoverflow.com/q/26955058/670433).

```` sh
heroku run bundle exec rake db:migrate
heroku run bundle exec rake db:seed
````

> NOTE: you may have to run these bundle commands within a `heroku run bash` prompt...

### Tasks

```` sh
heroku addons:create scheduler
heroku addons:open scheduler
````

### Mail

```` sh
heroku addons:create sendgrid:starter
heroku addons:open sendgrid
heroku config:get SENDGRID_USERNAME
heroku config:get SENDGRID_PASSWORD
````

## Debugging

````sh
heroku pg:info
heroku logs
heroku logs -t # for tail
heroku logs -n 1500
heroku run bash
heroku run console
heroku pg:backups capture
heroku ps  # get the process number, then stop with ...
heroku ps:stop scheduler.6531
````
