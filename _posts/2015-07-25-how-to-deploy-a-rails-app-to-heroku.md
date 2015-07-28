---
layout: post
title:  "How to deploy a Ruby on Rails app to a Heroku production server"
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
---

This document describes the process of deploying a Ruby on Rails application to a production server hosted by Heroku.

## Deployment

Download [heroku toolbelt](https://toolbelt.heroku.com/) to enable `heroku` command line tools.

```` sh
heroku login
cd ~/myapp
heroku create
heroku apps:rename myapp
# heroku domains:add example.com
heroku config:set KEY1=VALUE1 [KEY2=VALUE2 ...]
git push heroku master
````

## Database Management

Use the command line tools or the heroku console to manage environment variables.

```` sh
heroku pg:credentials DATABASE
heroku pg:reset DATABASE # equivalent to `rake db:drop && rake db:create`
heroku run bundle exec rake db:migrate
heroku run bundle exec rake db:seed
heroku pg:psql # log in to execute custom queries
````
