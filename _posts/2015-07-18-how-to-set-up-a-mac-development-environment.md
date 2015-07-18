---
layout: post
title:  "How to set up a mac development environment from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: false
icon_class: none
technologies: mac os-x chrome atom homebrew
---

This document describes the process of configuring a new Mac OS-X development environment from scratch.

## System Preferences
Configure hot-corners, keyboard shortcuts, keystroke settings.

## Downloads

[chrome](https://www.google.com/chrome/browser/desktop/index.html) web browser

[atom](https://atom.io/) text editor

[solarized](http://ethanschoonover.com/solarized) terminal theme

import the *osx-solarized.app-colors-solarized/Solarized Dark ansi.terminal* theme via Terminal settings.

[homebrew](http://brew.sh/)

```` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````

[pip](https://pip.pypa.io/en/stable/installing.html)

```` sh
brew install python
````

> python includes pip

[aws command line tools](http://aws.amazon.com/cli/)

Requires Python 2.6.5 or higher.
```` sh
pip install awscli
````


Create a new ssh key.
Overwrite bash profile.
Overwrite system config directories.
