---
layout: post
title:  "How to create a Ruby on Rails application"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: git ruby rubygems bundler rails rake bower
credits:
 - http://blog.blenderbox.com/2014/04/16/gitignore-everything-inside-a-directory/
---

This document describes the process of creating a new Ruby on Rails application from scratch, using Rails version 4.2.0.

## Generating

Create a new app and set up version control.

```` sh
bundle exec rails new MY_APP --database=DB_PROVIDER
cd MY_APP/
git init .
````

## Configuring Environment Variables

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
  secret_key_base: <%= ENV["MY_APP_SECRET_KEY_BASE"] %>

test:
  secret_key_base: <%= ENV["MY_APP_SECRET_KEY_BASE"] %>

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

## Installing Back-end Dependencies

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

Configure generators in **config/application.rb**.

```` rb
config.generators do |g|
  g.test_framework :rspec
  g.assets = false
  g.helper = false
end
````

## Installing Front-end Dependencies

Create a file called **package.json** in the root directory, optionally generating with `npm init`:

    {
      "name": "my-app",
      "version": "0.0.1",
      "dependencies": {
        "bower": "~1.4"
      },
      "engine": {
        "node": "0.12.7",
        "npm": "2.12.1"
      },
      "scripts": {
        "postinstall": "./node_modules/bower/bin/bower install"
      }
    }


Create a file called *bower.json* in the root directory, optionally generating with `bower init`:

    {
      "name": "my-app",
      "dependencies": {
        "bootstrap": "~3.3.4",
        "d3": "~3.5.5",
        "jquery": "~2.1.4",
        "jquery.ui": "~1.11.4",
        "octicons": "*"
      }
    }


Create a file called *.bowerrc* in the root directory of your app, to resemble the following template:

    {
      "directory": "vendor/assets/components"
    }

Install components with `bower install`. Alternatively run `npm install`, which, if **package.json** is configured according to the template above, will also perform a bower installation.

Require components in *app/assets/javascripts/application.js*:

    //= require d3/d3

> NOTE: place all component require statements above `//= require_tree .`

Remove installed components from source control by adding a file at *vendor/assets/components/.gitignore* with the following contents:

    [^.]*

## Configuring the Database

Ensure existence of the database user and password specified in **/config/database.yml**, using the command line or relational database management software as necessary.

Then create the database.

```` sh
bundle exec rake db:create
````

## Documenting

Create and edit **README.md**, **CREDITS.md**, and **LICENSE.md**. For the license, use this MIT template:

````
The MIT License (MIT)

Copyright (c) 2015 MY_NAME

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

````

## Writing

That's it. Now start coding your custom app.
