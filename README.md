resin-config-karma
==================

> Base Resin.io Karma Test Runner configuration

[![npm version](https://badge.fury.io/js/resin-config-karma.svg)](http://badge.fury.io/js/resin-config-karma)
[![dependencies](https://david-dm.org/resin-io-modules/resin-config-karma.svg)](https://david-dm.org/resin-io-modules/resin-config-karma.svg)
[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/resin-io/chat)

Installation
------------

Install `resin-config-karma` by running:

```sh
$ npm install --save-dev resin-config-karma
```

Setup
-----

This module aims to provide a base [Karma](https://karma-runner.github.io) configuration for Resin.io NPM modules.
It's preconfigured with the following assumptions about the modern resin.io modules structure:
- The distribution files are located in the `build` folder (typically created by building the TS or ES6+ sources)
and have the `.js` extension.
- The sources are located in `src` or `lib` folder and are in JS (ES) or TS.
- The test specs are located in the `tests` folder and have the double `.spec.js` or `.spec.ts` extension.
- The tests are importing the distribution or the source files.
It's recommended to simply import the root of the project and let `node` decide what to do, as in the real usage scenario.
It has the benefit of importing TS if the `types` entry is properly configured in `package.json`.

It supports the following features:
- Webpack transform for the tests bundle with ES2015 support and sourcemaps.
- Test are run in headless Chrome.
- SauceLabs configuration.
- Mocha support.
- CI mode enabled by default.

The module exposes a function returning the Karma configuration object
that you can pass to `config.set()` yourself.

To get started, create a `karma.conf.js` in the root of your project:

```js
var getKarmaConfig = require('resin-config-karma');
var packageJSON = require('./package.json');

module.exports = function(config) {
  var karmaConfig = getKarmaConfig(packageJSON /*, { wepbackConfig: optionalCustomWebpackConfig, slLaunchers: optionalCustomSauceLabsLaunchers }*/)

  // Notice you can override the default options as you wish
  karmaConfig.logLevel = config.LOG_INFO;

  config.set(karmaConfig);
};
```

And run `karma start`.

SauceLabs
---------

This configuration allows to easily setup your tests to run in SauceLabs by setting the following environment variables:

```sh
export SAUCE_LABS=true
export SAUCE_USERNAME=<username>
export SAUCE_ACCESS_KEY=<access key>
```

If `SAUCE_LABS` is not set, `karma` will only run the tests locally on Chrome.

Browsers
--------

The local tests run in the headless Chrome.

To run them locally you need Chrome >= 59 installed.

To run them on Travis add the following lines to your `.travis.yml` file:

```yaml
dist: trusty
sudo: false
addons:
  chrome: stable
```

*Note:* change `false` to `required` if you need sudo for your tests, see [the Travis Trusty docs](https://docs.travis-ci.com/user/reference/trusty/#Using-Trusty) and [the Travis container-based infrastructure docs](https://docs.travis-ci.com/user/migrating-from-legacy) for more details.

AppVeyor has the the proper Chrome version preinstalled.

The SauceLabs tests run in the browsers specified in [launchers.json](https://github.com/resin-io-modules/resin-config-karma/blob/master/launchers.json).

Support
-------

If you're having any problem, please [raise an issue](https://github.com/resin-io-modules/resin-config-karma/issues/new) on GitHub and the Resin.io team will be happy to help.
