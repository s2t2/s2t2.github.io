---
layout: post
title:  "How to create a Ruby on Rails application from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: ruby rails git
---

This document describes a process of creating a new rails application from scratch, using rails version 4.2.0.
It reflects the author's personal development preferences.

Substitute each apprearance of *MY_APP* with the name of your app, each appearance of *DB_PROVIDER* with your database provider name, and each appearance of *xyz123* with a unique string value.

## Create a new app and set up version control.

```` sh
bundle exec rails new MY_APP --database=DB_PROVIDER
cd MY_APP/
git init .
````

## Protect secrets.

Add `/config/secrets.yml` to **.gitignore** file and save it before making the initial commit:

```` sh
git add . # then double-check to make sure the config/secrets.yml didn't get checked-in.
git commit -m "creating a new rails app"
````

## Configure environment variables.

Edit **/config/database.yml** using the following template:

```` yaml
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root
  password: <%= ENV['DB_PROVIDER_ROOT_PASSWORD'] %>
  socket: /tmp/mysql.sock

development:
  <<: *default
  database: MY_APP_development

test:
  <<: *default
  database: MY_APP_test

production:
  <<: *default
  database: MY_APP_production
  username: MY_APP
  password: <%= ENV['MY_APP_DATABASE_PASSWORD'] %>

````

Edit **config/secrets.yml** using the following template:

```` yaml
development:
  secret_key_base: xyz123

test:
  secret_key_base: xyz123

production:
  secret_key_base: <%= ENV["MY_APP_SECRET_KEY_BASE"] %>
````

<!--aside class="notice">
  Is there any reason not to reuse the same environment variable for secret key base across all environments?
</aside-->

Finally, edit **/.bash_profile** (or **/.bashrc**) to ensure existence of the variables referenced above.

```` sh
export DB_PROVIDER_ROOT_PASSWORD="xyz123"
export MY_APP_DATABASE_PASSWORD="xyz123"
export MY_APP_SECRET_KEY_BASE="xyz123"
````
> After editing the bash profile, you may have to open a new terminal window for the changes to take effect.

Commit changes to version control.

```` sh
git commit -am "configuring environment variables"
````

## Install and configure gems.

Revise **Gemfile** using the following template, commenting-out the [rubygems](https://rubygems.org/) you won't be using:

```` rb
source 'https://rubygems.org'

gem 'rails', '4.2.0'
gem 'mysql2' # or 'pg' based on your preferred DB_PROVIDER
# gem 'sass-rails', '~> 5.0'
gem 'uglifier', '>= 1.3.0'
# gem 'coffee-rails', '~> 4.1.0'
# gem 'therubyracer', platforms: :ruby

gem 'jquery-rails'
gem 'turbolinks'
gem 'jbuilder', '~> 2.0'
gem 'sdoc', '~> 0.4.0', group: :doc

group :development, :test do
  # gem 'byebug'
  gem 'web-console', '~> 2.0'
  gem 'spring'
  gem 'pry'
  gem 'rspec-rails', '~> 3.0'
end
````
> The important additions are 'pry' for debugging and 'rspec-rails' for testing.

Install gems via *bundler*:

```` sh
bundle install
````

Finish rspec installation. Refer to [rspec installation instructions](https://github.com/rspec/rspec-rails#installation) as necessary.

```` sh
bundle exec rails generate rspec:install
````

Remove tests directory.

```` sh
rm -rf test/
````

Commit changes to version control.

```` sh
git commit -am "installing and configuring gems"
````
## Configure generators.

Configure generators in **config/application.rb**.

```` rb
config.generators do |g|
  g.test_framework :rspec
  g.assets = false
  g.helper = false
end
````

Commit changes to version control.

```` sh
git commit -am "configuring generators"
````

## Create database.

Ensure existence of the database user and password specified in **/config/database.yml**, using the command line or relational database management software as necessary.

Then create the database.

```` sh
bundle exec rake db:create
````

## Wrapping up

That's it. Now start coding your custom app.
