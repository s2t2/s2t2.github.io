---
layout: post
title:  "How to set up development environment (mac os-x)"
author: MJ Rossetti
categories: process-documentation
tags: none
published: false
icon_class: none
technologies: mac os-x chrome atom homebrew
---

This document describes the process of configuring a new Mac OS-X development environment from scratch.

## System Preferences

Configure hot-corners, keyboard shortcuts, keystroke settings, etc.

## Browser

Install [chrome](https://www.google.com/chrome/browser/desktop/index.html) web browser.

## Text Editor

Install [atom](https://atom.io/) text editor. Install [sync-settings](https://github.com/Hackafe/atom-sync-settings), specify your github access token and gist id to restore text editor settings. If you are keeping this information in your bash profile, is likely you may need to recover your profile before performing the sync.

## Terminal

[solarized](http://ethanschoonover.com/solarized) terminal theme

import the *osx-solarized.app-colors-solarized/Solarized Dark ansi.terminal* theme via Terminal settings.

## Homebrew

[homebrew](http://brew.sh/)

```` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````

## Ruby, Rbenv, and Bundler

As a developer working on more than one ruby project, it sometimes becomes necessary to specify different ruby versions for each project. Rbenv makes switching ruby versions easy. Ruby-build facilitates the installation of ruby versions.

```` sh
brew install rbenv
brew install ruby-build
brew install rbenv-bundler
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
# gem install rails --nodoc
````

## Bash Profile and Config (Dotfiles)
















### Python

[pip](https://pip.pypa.io/en/stable/installing.html)

```` sh
brew install python
brew linkapps python
````

## awscli

[aws command line tools](http://aws.amazon.com/cli/) -- Requires Python 2.6.5 or higher.
```` sh
pip install awscli
````



Create a new ssh key.
Overwrite bash profile.
Overwrite system config directories.


# Credits, Notes, and References
 + http://octopress.org/docs/setup/rbenv/
