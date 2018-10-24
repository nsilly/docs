# Logging

- [Logging](#logging)
  - [Introduction](#introduction)
  - [Configuration](#configuration) - [Configuring](#configuring) - [Log Levels](#log-levels)
  - [Writing Log Messages](#writing-log-messages) - [Contextual Information](#contextual-information)
    - [Writing To Specific Channels](#writing-to-specific-channels)
  - [Advanced Monolog Channel Customization](#advanced-monolog-channel-customization)
    - [Customizing Monolog For Channels](#customizing-monolog-for-channels)
    - [Creating Monolog Handler Channels](#creating-monolog-handler-channels)
      - [Monolog Formatters](#monolog-formatters)
    - [Creating Channels Via Factories](#creating-channels-via-factories)

<a name="introduction"></a>

## Introduction

To help you learn more about what's happening within your application, nsilly provides robust logging services that allow you to log messages to files, the system error log, and even send it via several transporter.

Under the hood, nsilly utilizes the [winston](https://github.com/winstonjs/winston) library, which provides support for a variety of powerful log handlers.

<a name="configuration"></a>

## Configuration

All of the configuration for your application's logging system is housed in the `config/logging.js` configuration file. This file allows you to configure your application's log transporter, so be sure to review each of the available transporter and their options. Of course, we'll review a few common options below.

By default, nsilly will use the `console` transporter in development when logging messages.

The log adapters is registered to application in `app.js` that located in root directiory

#### Configuring

```
import { ColorfulConsoleAdapter } from '@nsilly/log';

let adapters = [];
if (process.env.APP_ENV === 'production') {
  adapters = [];
} else {
  adapters = [new ColorfulConsoleAdapter()];
}

export default adapters;
```

#### Log Levels

The Logger offers all of the log levels defined in the [RFC 5424 specification](https://tools.ietf.org/html/rfc5424): **emergency**, **alert**, **critical**, **error**, **warning**, **notice**, **info**, and **debug**.

So, imagine we log a message using the `debug` method:

```javascript
import { App } from "@nsilly/container";
import { Logger } from "@nsilly/log";

App.make(Logger).debug("An informational message.");
```

<a name="writing-log-messages"></a>

## Writing Log Messages

You may write information to the logs using

```javascript
App.make(Logger).emergency(message);
App.make(Logger).alert(message);
App.make(Logger).critical(message);
App.make(Logger).error(message);
App.make(Logger).warning(message);
App.make(Logger).notice(message);
App.make(Logger).info(message);
App.make(Logger).debug(message);
```

#### Contextual Information

An object of contextual data may also be passed to the log methods. This contextual data will be formatted and displayed with the log message:

```javascript
App.make(Logger).info("User failed to login.", {
  id: 1,
  email: "user@example.com"
});
```
