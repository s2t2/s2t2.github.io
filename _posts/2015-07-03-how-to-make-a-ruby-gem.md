---
layout: post
title:  "How to create a Ruby gem from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: ruby rubygems bundler rake rails
credits:
  - http://guides.rubygems.org/make-your-own-gem/
  - http://rakeroutes.com/blog/lets-write-a-gem-part-one/
  - http://rakeroutes.com/blog/lets-write-a-gem-part-two/
  - http://elgalu.github.io/2013/tools-for-creating-your-first-ruby-gem/
  - http://www.alexedwards.net/blog/how-to-make-a-rubygem-part-two
---

This document describes the process of creating and managing a [Ruby gem](https://rubygems.org/).

## Generating

```` sh
bundle gem mygem
cd mygem
````

## Describing

Update the summary, description, and homepage attributes in *mygem.gemspec*.

Update the "Usage" section in *README.md* to describe desired functionality.

Update the "Contributing" section in *README.md* according to the following template:

    ## Contributing

    Browse existing issues or create a new issue to communicate bugs, desired features, etc.

    After forking the repo and pushing your changes, create a pull request referencing the applicable issue(s).

    ### Installation

    Check out the repo with `git clone git@github.com:${YOUR_GITHUB_USERNAME}/${GEM_REPO_NAME}.git`, and `cd ${GEM_REPO_NAME}`.

    After checking out the repo, run `bin/setup` to install dependencies.

    ### Testing

    Run `bundle exec rake` or `bundle exec rspec spec/` to run the tests.

    You can also run `bin/console` for an interactive prompt that will allow you to experiment.

    To install this gem onto your local machine, run `bundle exec rake install`.

    ### Releasing

    Update the version number in `version.rb`, then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Writing

Make the library do what you said it would do. Revise *README.md* as necessary.

Have fun!

## Testing

The new gem generation process prompts you to choose a test suite, and will configure much of it for you. Check your configuration using the steps below as necessary.

### Configuring Rspec

Revise *mygem.gemspec* to include the following development dependencies:

    spec.add_development_dependency "rspec", "~> 3.1"
    spec.add_development_dependency "pry", "~> 0.10"

Install the dependencies with `bundle install` then initialize rspec with `rspec --init`.

Add these lines to the top of *spec/spec_helper.rb* to facilitate reference to your gem's classes and methods during testing:

    $LOAD_PATH.unshift File.expand_path('../../lib', __FILE__)
    require 'mygem'
    require 'pry'

### Writing Tests

Create an rspec test (*spec/myclass_spec.rb*) using the following template:

    require 'spec_helper'

    Mygem
      RSpec.describe Myclass do
        describe '#all' do
          it "does some stuff" do
            expect(true)
          end
        end
      end
    end


### Running Tests

```` sh
bundle exec rspec spec/
````

## Debugging

### Default Console

The new gem generation process provides an interactive console that auto-loads the gem library for you. Configure *bin/console* to use pry instead of IRB, then start it up.

````
#!/usr/bin/env ruby

require "bundler/setup"
require "mygem"

# You can add fixtures and/or initialization code here to make experimenting
# with your gem easier. You can also use a different console, if you like.

# (If you use this, don't forget to add pry to your Gemfile!)
require "pry"
Pry.start

# require "irb"
# IRB.start
````

```` sh
bin/console
````

### Alternative Consoles

Alternatively install, load, and test the gem locally using other consoles.

```` sh
bundle exec rake install
````

#### IRB:

```` sh
irb -Ilib -rmygem # will auto-load the gem in a new IRB console
````

#### Rails:

Reference local gem installation from *Gemfile*, then install the gem before starting a console.

```` rb
gem 'mygem', '~> 0.0.1', :path => "../mygem"
````

```` sh
bundle install
rails console
````

## Documenting

Write comments above public-facing methods according to the [YARD](http://yardoc.org/) specification.

Revise *mygem.gemspec* to include the following development dependency:

    spec.add_development_dependency "yard"

After installing with `bundle install`, run `bundle exec yard doc` to parse comments and/or `bundle exec yard server` to view documentation at *localhost:8808*.

## Versioning and Releasing

Edit gem version in *lib/mygem/version.rb*.

Run `bundle exec rake release` to auto-generate git tag and push to github and rubygems.
