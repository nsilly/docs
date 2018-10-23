# Deployment

- [Deployment](#deployment)
    - [Introduction](#introduction)
    - [Compile](#compile)
    - [Start Production Server](#start-production-server)
    - [Server Configuration](#server-configuration)
        - [Nginx](#nginx)

<a name="introduction"></a>

## Introduction

When you're ready to deploy your application to production, there are some important things you can do to make sure your application is running as efficiently as possible. In this document, we'll cover some great starting points for making sure your application is deployed properly.

<a name="compile"></a>

## Compile

The framework is wrote with ES6, we are using [Babel](https://babeljs.io/) to run the application locally. But it cost more resource and slower if we using Babel on production. That's why we need to compile application to ES2015 and run it with Nodejs

By run the following command we will compile the application to ES2015, all babel configuration can be found in `.babelrc` that located in root directory.

The compiled files are located in `dist` folder.

```
yarn build
```

<a name="start-production-server"></a>

## Start Production Server

To run the application on production we need something to keep a child process running continuously and automatically restart it when it exits unexpectedly.

We can use [forever](https://github.com/foreverjs/forever). A simple CLI tool for ensuring that a given script runs continuously.


<a name="server-configuration"></a>

## Server Configuration

<a name="nginx"></a>

### Nginx

If you are deploying your application to a server that is running Nginx and you want to map the port 80 to the application's port, you may use the following configuration file as a starting point for configuring your web server. Most likely, this file will need to be customized depending on your server's configuration.

```
    server {
        listen 80;
        server_name example.com;

        add_header Cache-Control no-cache;
        client_max_body_size 100m;

        charset utf-8;

        location / {
            proxy_pass http://192.168.10.1:3000;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection "upgrade";
        }

        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }

        location ~ /\.(?!well-known).* {
            deny all;
        }
    }
```
