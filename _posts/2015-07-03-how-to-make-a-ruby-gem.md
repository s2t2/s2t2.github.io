---
layout: post
title:  "How to create a Ruby gem from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: ruby rubygems bundler rake
---

This document describes a process of creating a new Ruby gem (open source library) from scratch, using bundler. It is a compilation of [rubygems guides](http://guides.rubygems.org/) and personal practices.

## Generate a new gem using bundler.

```` sh
bundle gem twenty_sixteen
cd mygem
````

## Edit the gem's info.

Update the "Usage" section in *README.md* to describe desired functionality.

Update at least the summary, description, and homepage attributes in *mygem.gemspec* .

## Write the gem's logic.

Make the code library do what you said it would do in the README documentation. Revise the README if necessary. Have fun!

## Debug and test the gem.

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
require 'spec_helper'

RSpec.describe Myclass do
  describe '#all' do
    it "does some stuff" do
      expect(true)
    end
  end
end
````

Run tests...

```` sh
bundle exec rspec spec/
````
... or open a ruby console to test manually.

```` sh
irb -Ilib -rmygem
````

## Release the gem.

Build the gem.

```` sh
gem build mygem.gemspec
gem install mygem-0.0.1.gem
````

Push gem to rubygems, auto-generate git tag, and release.

```` sh
rake release
````
