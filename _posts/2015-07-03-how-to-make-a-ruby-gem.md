---
layout: post
title:  "How to create a Ruby gem from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: ruby rubygems bundler rake rails
---

This document describes the process of creating and managing a Ruby gem (open source library).

## Generating

```` sh
bundle gem mygem
cd mygem
````

## Describing

Update the "Usage" section in *README.md* to describe desired functionality.

Update at least the summary, description, and homepage attributes in *mygem.gemspec* .

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

Create an rspec test using the following template:

```` rb
# spec/myclass_spec.rb

RSpec.describe Myclass do
  describe '#all' do
    it "does some stuff" do
      expect(true)
    end
  end
end
````

Run tests.

```` sh
bundle exec rspec spec/
````

## Debugging

Build and install the gem locally.

```` sh
rake install
````

Then require and test it within a ruby console.

### Using IRB:

```` sh
irb -Ilib -rmygem
````

### Using Rails:

Reference local gem installation from *Gemfile*.

```` rb
gem 'mygem', '~> 0.0.1', :path => "../mygem"
````

Install gems.

```` sh
bundle install
rails console
````

## Versioning and Releasing

Edit gem version in *lib/mygem/version.rb*.

Push gem to rubygems, auto-generate git tag, and release.

```` sh
rake release
````
