# Configuration

- [Configuration](#configuration)
    - [Introduction](#introduction)
    - [Environment Configuration](#environment-configuration)
        - [Environment Variable Types](#environment-variable-types)
        - [Retrieving Environment Configuration](#retrieving-environment-configuration)

<a name="introduction"></a>
## Introduction

All of the configuration files for the framework are stored in the `config` directory. Each option is documented, so feel free to look through the files and get familiar with the options available to you.

<a name="environment-configuration"></a>
## Environment Configuration

It is often helpful to have different configuration values based on the environment where the application is running. For example, you may wish to use a different cache driver locally than you do on your production server.

To make this a cinch, nsily use the [DotEnv](https://github.com/motdotla/dotenv) that loads environment variables from a `.env` file into `process.env`. 

Your `.env` file should not be committed to your application's source control, since each developer / server using your application could require a different environment configuration. Furthermore, this would be a security risk in the event an intruder gains access to your source control repository, since any sensitive credentials would get exposed.

If you are developing with a team, you may wish to continue including a `.env.example` file with your application. By putting place-holder values in the example configuration file, other developers on your team can clearly see which environment variables are needed to run your application. 

<a name="environment-variable-types"></a>
### Environment Variable Types

> All variables in your `.env` files are parsed as strings.

If you need to define an environment variable with a value that contains spaces, you may do so by enclosing the value in double quotes.

```
APP_NAME="My Application"
```

<a name="retrieving-environment-configuration"></a>
### Retrieving Environment Configuration

All of the variables listed in this file will be loaded into the `process.env`.

```javascript
const app_name = process.env.APP_NAME;
```
