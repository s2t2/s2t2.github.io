---
layout: post
title:  "How to set up a Mac OS-X development environment from scratch"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: mac os-x chrome atom homebrew ruby bundler python lunchy postgresql mysql
credits:
  - http://octopress.org/docs/setup/rbenv/
  - https://help.github.com/articles/associating-text-editors-with-git/#using-atom-as-your-editor
  - http://stackoverflow.com/a/28142874/670433
  - http://www.moncefbelyamani.com/how-to-install-postgresql-on-a-mac-with-homebrew-and-lunchy/
  - http://blog.willj.net/2011/05/31/setting-up-postgresql-for-ruby-on-rails-development-on-os-x/
  - http://stackoverflow.com/a/17936043/670433
  - http://www.postgresql.org/docs/current/interactive/app-psql.html
  - http://www.postgresql.org/docs/7.0/static/security.htm
  - http://www.cyberciti.biz/faq/howto-add-postgresql-user-account/
  - http://dotwell.io/taking-advantage-of-bower-in-your-rails-4-app/
---

This document describes the process of configuring a new Mac OS-X development environment from scratch.

## System Preferences

Configure hot-corners, [keyboard shortcuts](http://lifehacker.com/343328/create-a-keyboard-shortcut-for-any-menu-action-in-any-program), keystroke settings, etc.

## Browser

Install [chrome](https://www.google.com/chrome/browser/desktop/index.html) web browser.

## Text Editor

Install [atom](https://atom.io/) text editor. Install [sync-settings](https://github.com/Hackafe/atom-sync-settings), specify your github access token and gist id to restore text editor settings.

## Terminal

Download [solarized](http://ethanschoonover.com/solarized) terminal theme.

Import the *osx-solarized.app-colors-solarized/Solarized Dark ansi.terminal* theme via terminal settings.

## Homebrew

Install [homebrew](http://brew.sh/) package manager.

```` sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````

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
gem install bundler
gem install rake --version='10.4.2'
gem install rdoc -v '4.2.0'
````

### Python

```` sh
brew install python
brew linkapps python
````

### Node

```` sh
brew install node
````

#### Bower

```` sh
cd my_app/
npm install -g bower
# /usr/local/bin/bower -> /usr/local/lib/node_modules/bower/bin/bower
# bower@1.4.1 /usr/local/lib/node_modules/bower
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

## Databases

Use [`lunchy`](https://github.com/eddiezane/lunchy) gem to manage services/daemons, including database servers.

```` sh
gem install lunchy
````

Restart your terminal.

### PostgreSQL

Install.

```` sh
brew install postgresql
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents # to have launchd start postgresql at login
lunchy start postgresql
createdb # to create a database named after your root database user, which is named after your mac username; avoids `psql: FATAL:  database $USER does not exist`
````

Set a password for the database root user.

````
psql -U $USER -c "ALTER USER $USER WITH PASSWORD 'CHANGE_ME';"
````

To require password authentication, find the location of the *pg_hba.conf* file using `psql -U $USER -c "SHOW hba_file;"`, and edit it to resemble to the following template:

    # TYPE  DATABASE        USER            ADDRESS                 METHOD
    local   all             all                                     md5
    host    all             all             127.0.0.1/32            md5
    host    all             all             ::1/128                 md5

Basically you are just changing the *methods* from `trust` to `md5`. Restart the server to apply the authentication changes.

```` sh
lunchy restart postgresql
psql -U $USER # you should now be prompted for a password
````

Helpful psql commands and their SQL equivalents:

```` sql
SELECT * FROM pg_user;
-- or... `\du`

SELECT * FROM pg_database;
-- or... `\l`

SELECT * FROM pg_table WHERE schema_name = 'my_db';
-- or... `\connect my_db` and `\dt`
````

Optionally install the pg gem/driver if you are going to be connecting with a Ruby on Rails app. Find the *pg_config* file with `psql -U $USER -c "SHOW config_file;"`, navigate to .

```` sh
gem install pg -- --with-pg-config=/usr/local/bin/pg_config
````

Optionally create an application database and database user.

```` rb
CREATE USER app_user WITH password 'CHANGE_ME';
ALTER USER app_user CREATEDB;
ALTER USER app_user WITH SUPERUSER;
CREATE DATABASE app_db;
GRANT ALL PRIVILEGES ON DATABASE app_db to app_user;
````

Finally, install [pgAdmin](http://www.pgadmin.org/download/macosx.php) database management software. Specify your root database user credentials when connecting to the local database server.

### MySQL

Install.

```` sh
brew install mysql
````

## Graphing Library

If using the [`rails-erd`](https://github.com/voormedia/rails-erd) gem, satisfy [`ruby-graphviz`](https://github.com/glejeune/Ruby-Graphviz) dependency by installing the [`graphviz`](http://graphviz.org/) library.

```` sh
brew install graphviz
````
