---
layout: post
title:  "How to set up a Mac OS-X development environment from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: mac os-x chrome atom homebrew
credits:
  - http://octopress.org/docs/setup/rbenv/
  - https://help.github.com/articles/associating-text-editors-with-git/#using-atom-as-your-editor
  - http://stackoverflow.com/a/28142874/670433
---

This document describes the process of configuring a new Mac OS-X development environment from scratch.

## System Preferences

Configure hot-corners, keyboard shortcuts, keystroke settings, etc.

## Browser

Install [chrome](https://www.google.com/chrome/browser/desktop/index.html) web browser.

## Text Editor

Install [atom](https://atom.io/) text editor. Install [sync-settings](https://github.com/Hackafe/atom-sync-settings), specify your github access token and gist id to restore text editor settings.

## Terminal

Download [solarized](http://ethanschoonover.com/solarized) terminal theme.

Import the *osx-solarized.app-colors-solarized/Solarized Dark ansi.terminal* theme via terminal settings.

## Package Manager

Install [homebrew](http://brew.sh/) package manager.

```` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````

## Programming Langugages

### Ruby

As a developer working on more than one ruby project, it sometimes becomes necessary to specify different ruby versions for each project. Rbenv makes switching ruby versions easy. Ruby-build facilitates the installation of ruby versions.

```` sh
brew install rbenv
brew install ruby-build
````

To use Homebrew's directories rather than ~/.rbenv add to your profile: `export RBENV_ROOT=/usr/local/var/rbenv`

To enable shims and autocompletion add to your profile: `if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi`

Restart your terminal for the profile changes to take place.

```` sh
rbenv install 2.2.0 # to install a specific ruby version from the internet
rbenv rehash # run this after installing a new ruby version
rbenv global 2.2.0 # to set a specific ruby version for use
rbenv bundler on # to obviate need to type 'bundle exec' before rake and other commands
gem install bundler
````

### Python

```` sh
brew install python
brew linkapps python
````

## Configuration Files and Credentials

Use the [`s3_sync`](https://github.com/s2t2/s3-sync-ruby) ruby gem to recover files from s3. This requires you to obtain the configuration variables used during previous syncs.

```` sh
gem install "s3_sync"
````

```` rb
require "s3_sync"

S3Sync.configure do |config|
  config.key_id = ENV["AWS_S3_KEY_ID"]
  config.key_secret = ENV["AWS_S3_KEY_SECRET"]
  config.region = ENV["AWS_S3_REGION"]
  config.bucket = ENV["AWS_S3_BUCKET"]
  config.secret_phrase = ENV["AWS_S3_ENCRYPTION_PHRASE"]
  config.files = [
    File.join(Dir.home,".bash_profile"),
    File.join(Dir.home,".ssh","config")
  ]
end

S3Sync.download.new
````

Configure [Rubygems credentials](https://rubygems.org/profile/edit), typing your password when prompted:

```` sh
curl -u $YOUR_RUBYGEMS_USERNAME https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
````

Configure [Git credentials](https://help.github.com/categories/setup/):

```` sh
git config user.name $YOUR_GITHUB_USERNAME
git config user.email $YOUR_GITHUB_EMAIL
````

Generate [new ssh keys](https://help.github.com/articles/generating-ssh-keys/#step-2-generate-a-new-ssh-key), and copy the public key to GitHub and other hosts:

```` sh
ssh-keygen -t rsa -b 4096 -C $YOUR_SSH_EMAIL
pbcopy < ~/.ssh/id_rsa.pub
````
