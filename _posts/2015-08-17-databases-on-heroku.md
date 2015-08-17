---
layout: post
title:  "Working with databases on Heroku"
author: MJ Rossetti
categories: process-documentation
tags: none
published: true
icon_class: none
technologies: heroku mysql postgresql ssh sequel-pro
credits :
  - http://stackoverflow.com/questions/9822313/remote-connect-to-cleardb-heroku-database
  - https://getsatisfaction.com/cleardb/topics/unable_to_download_cert_error_207
  - https://getsatisfaction.com/cleardb/topics/mysql_said_ssl_connection_error
---

The process differs, depending on the database provider (MySQL vs PostgreSQL), and your operating system (Mac OS or Windows OS).

## MySQL

### Create Database

```` sh
heroku addons:create cleardb:ignite
````

### Connect to Database

In the Heroku application management console, find the Heroku config variable called `CLEARDB_DATABASE_URL`, which will resemble `mysql://user:pass@host.com/database?reconnect=true`.

In the ClearDB resource management console, navigate to the "Endpoint Information" page and locate Access Credentials: *Username* and *Password*.

In the ClearDB resource management console, locate SSL Certificates: *ClearDB CA Certificate*, *Client Certificate*, and *Client Private Key*. Download the files, using the **Save File** option.

> NOTE: If you receive a message like `The server returned an invalid certificate. Error 207 (net::ERR_CERT_INVALID).`, then try downloading using Firefox or Safari (not Chrome).

#### Mac OS

Move the certificate files into a subdirectory of your `~/.ssh` directory for future reference and usage.

> NOTE: For this example we will use the `~/.ssh/gwu-badm-2301/` directory.

```` sh
mkdir -p ~/.ssh/gwu-badm-2301/
ls -ltr ~/Downloads

cat ~/Downloads/cleardb-ca.pem
cat ~/Downloads/abc123-cert.pem
cat ~/Downloads/abc123-key.pem

mv ~/Downloads/cleardb-ca.pem ~/.ssh/gwu-badm-2301/
mv ~/Downloads/abc123-cert.pem ~/.ssh/gwu-badm-2301/
mv ~/Downloads/abc123-key.pem ~/.ssh/gwu-badm-2301/

ls -la ~/.ssh/gwu-badm-2301/
````

Modify SSL Key

```` sh
cd ~/.ssh/gwu-badm-2301/
openssl rsa -in abc123-key.pem -out abc123-key-no-password.pem
````

Download and install [Sequel Pro](http://www.sequelpro.com/download).

In Sequel Pro, create a new **Standard** connection using the following credentials:

Attribute Name | Attribute Value (or instructions)
--- | ---
Name | for personal reference
Host | obtain from `CLEARDB_DATABASE_URL`
Username | obtain from "Endpoint Information" page
Password | obtain from "Endpoint Information" page
Database | obtain from `CLEARDB_DATABASE_URL`
Port | 3306 (or leave blank)
Connect using SSL | true (check the box)
Key File | choose the 'abc123-key-no-password.pem' file
Certificate | choose the `abc123-cert.pem` file
CA Cert | choose the `cleardb-ca.pem` file

> PRO-TIP: To avoid re-entering this configuration upon each subsequent connection, save the configuration to favorites, test the connection, and save again before connecting for the first time.

> NOTE: To select certificate files, click the key file selector button, check "show hidden files" and navigate to `~/.ssh/gwu-badm-2301/`.

#### Windows OS

Download todo.

## PostgreSQL

### Create Database

```` sh
heroku addons:create heroku-postgresql:hobby-dev
````

### Connect to Database

#### Mac OS

Download todo.

#### Windows OS

Download todo.
