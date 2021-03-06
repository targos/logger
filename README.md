<div align="center">
  <img src="https://res.cloudinary.com/adonisjs/image/upload/q_100/v1564392111/adonis-banner_o9lunk.png" width="600px">
</div>

# Logger
[![circleci-image]][circleci-url] [![npm-image]][npm-url] ![][typescript-image] [![license-image]][license-url]

Extremely fast JSON logger built on top of [pino](http://getpino.io). The module does handful of modifications to the config of pino and also exposes a fake logger to be used during tests.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of contents

- [Usage](#usage)
- [Usage with AdonisJs](#usage-with-adonisjs)
- [What's changed from Pino?](#whats-changed-from-pino)
  - [Making certain fields required](#making-certain-fields-required)
    - [enabled](#enabled)
    - [name](#name)
    - [level](#level)
  - [Modifications](#modifications)
    - [`changeLevelName -> levelKey`](#changelevelname---levelkey)
    - [stream](#stream)
- [Fake logger](#fake-logger)
- [API](#api)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Usage
Install the package from npm as follows:

```sh
npm i @adonisjs/logger

# yarn
yarn add @adonisjs/logger
```

and then use it as follows

```ts
import { Logger } from '@adonisjs/logger/build/standalone'
const logger = new Logger({
  enabled: true,
  name: 'adonis-logger',
  level: 'debug',
})

logger.debug('received new request %s', request.url())
```

## Usage with AdonisJs
The `@adonisjs/core` module includes this module by default. However, here's how you can set it up manually.

```ts
const providers = [
  '@adonisjs/logger'
]
```

And then also register the typings file inside `tsconfig.json` file.

```json
{
  "files": ["./node_modules/@adonisjs/logger/build/adonis-typings/logger.d.ts"]
}
```

## What's changed from Pino?
We have changed handful of config options to make things more explicit. The modifications are based on our own opinions.

### Making certain fields required
All config variables are optional in pino, however, we want to make following config options required, so that the user of the logger always knows, where the values are coming from.

#### enabled
Making it clear, that logger can be disabled (if required).

#### name
Seeing logs without knowing their origin isn't really helpful. We force to define the name of the application.

#### level
The level at which you want to report must be decided upfront

### Modifications

#### `changeLevelName -> levelKey`
Just like the `messageKey`, you can define the key for showing the level number inside the logs. For some reasons pino call this option `changeLevelName`, which sounds more like an action over a configuration. We have renamed the option to `levelKey`.

#### stream
Instead of passing the stream as the 2nd argument, you can define the custom stream within the config.

```ts
import { Logger } from '@adonisjs/logger/build/standalone'

new Logger({
  stream: process.stdout,
})
```

## Fake logger
Many times you would want to test whether your code is logging certain messages or not. One way is to hijack the `stdout` stream and read the rows text from it.

Instead, we ship with a proper `FakeLogger` that you can use during tests.

```ts
export class MyApp {
  constructor (logger) {
    this.logger = logger
  }

  perform () {
    this.logger.debug('created app')
  }
}
```

Inside your tests

```ts
import { FakeLogger } from '@adonisjs/logger/build/standalone'
import { MyApp } from './MyApp'

const logger = new FakeLogger({
  // config
})

const app = new MyApp(logger)
app.perform()

assert.equal(logger.logs[0].msg, 'created app')
assert.equal(logger.logs[0].level, 20)

// Exists on fake logger only
logger.clear()
```

> The fake logger works seamless with the child logger as well. The root logger will collect all the logs from nested child loggers.

## API
Following are the autogenerated files via Typedoc

* [API](docs/README.md)


[circleci-image]: https://img.shields.io/circleci/project/github/adonisjs/logger/master.svg?style=for-the-badge&logo=circleci
[circleci-url]: https://circleci.com/gh/adonisjs/logger "circleci"

[typescript-image]: https://img.shields.io/badge/Typescript-294E80.svg?style=for-the-badge&logo=typescript
[typescript-url]:  "typescript"

[npm-image]: https://img.shields.io/npm/v/@adonisjs/logger.svg?style=for-the-badge&logo=npm
[npm-url]: https://npmjs.org/package/@adonisjs/logger "npm"

[license-image]: https://img.shields.io/npm/l/@adonisjs/logger?color=blueviolet&style=for-the-badge
[license-url]: LICENSE.md "license"
