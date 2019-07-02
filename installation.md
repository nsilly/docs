# Installation

- [Installation](#installation)
    - [Server Requirements](#server-requirements)
    - [Installing](#installing)
        - [Via Installer](#via-installer)
        - [Clone on github](#clone-on-github)
    - [Local Development Server](#local-development-server)
    - [Configuration](#configuration)
        - [Configuration Files](#configuration-files)
        - [Environments Variables](#environments-variables)
        - [Additional Configuration](#additional-configuration)

<a name="server-requirements"></a>

## Server Requirements

The nsilly framework has a few system requirements. You will need to make sure your server meets the following requirements:

<div class="content-list" markdown="1">
- Nodejs >= v8.0
</div>

<a name="installing"></a>

## Installing

<a name="via-installer"></a>

### Via Installer

> Comming soon !

<a name="clone-on-github"></a>

### Clone on github

```
git clone https://github.com/nsilly/nsilly
```

<a name="local-development-server"></a>

## Local Development Server

If you have Nodejs installed locally and you would like to start your development server:

```
yarn dev
```

<a name="configuration"></a>

## Configuration

<a name="configuration-files"></a>

### Configuration Files

All of the configuration files for the framework are stored in the `config` directory. Each option is documented, so feel free to look through the files and get familiar with the options available to you.

<a name="environments-variables"></a>

### Environments Variables

nislly load environment variables from a `.env` file that located in root directory. You can create a file by yourself with our example `.env.example`

<a name="additional-configuration"></a>

### Additional Configuration

nsilly needs almost no other configuration out of the box. You are free to get started developing! However, you may wish to review the `config/sequelize.js` file and its documentation. It contains several options for [Sequelize](http://docs.sequelizejs.com).

You may also want to configure a few additional components of nsilly, such as:

<div class="content-list" markdown="1">
- [Database](/docs/{{version}}/database#configuration)
- [Logging](/docs/{{version}}/logging#configuration)
</div>
