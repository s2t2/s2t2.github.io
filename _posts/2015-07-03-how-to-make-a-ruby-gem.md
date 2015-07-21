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
  - http://rakeroutes.com/blog/lets-write-a-gem-part-one/
  - http://rakeroutes.com/blog/lets-write-a-gem-part-two/
---

This document describes the process of creating and managing a Ruby gem (open source library).

## Generating

```` sh
bundle gem mygem
cd mygem
````

## Describing

Update at least the summary, description, and homepage attributes in *mygem.gemspec*.

Update the "Usage" section in *README.md* to describe desired functionality.

Update the "Contributing" section in *README.md* according to the following template:
```` md
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
````

## Writing

Make the code library do what you said it would do in the README documentation. Revise the README if necessary. Have fun!

## Testing

Revise *mygem.gemspec* to include the following development dependencies:

```` rb
spec.add_development_dependency "rspec", "~> 3.1"
spec.add_development_dependency "pry", "~> 0.10"
````

Install the dependencies with `bundle install` then initialize rspec with `rspec --init`.

Add this line to the top of *spec/spec_helper.rb* to facilitate reference to your gem's classes and methods:

```` rb
require 'mygem'
````

Create an rspec test (*spec/myclass_spec.rb*) using the following template:

```` rb
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
````

Run tests.

```` sh
bundle exec rspec spec/
````

## Debugging

### Default Console

Newly-generated ruby gems come with an interactive console that auto-loads the gem library for you. Configure *bin/console* to use pry instead of IRB, then start it up.

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

## Versioning and Releasing

Edit gem version in *lib/mygem/version.rb*.

Push gem to rubygems, auto-generate git tag, and release.

```` sh
bundle exec rake release
````
