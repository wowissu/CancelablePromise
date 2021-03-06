# cancelable-promise

[![GitHub license](https://img.shields.io/github/license/alkemics/CancelablePromise)](https://github.com/alkemics/CancelablePromise/blob/master/LICENSE) [![npm version](https://img.shields.io/npm/v/cancelable-promise)](https://www.npmjs.com/package/cancelable-promise) [![Node.js CI](https://github.com/alkemics/CancelablePromise/workflows/Node.js%20CI/badge.svg?branch=master)](https://github.com/alkemics/CancelablePromise/actions?query=workflow%3A%22Node.js+CI%22) [![End-to-end tests](https://github.com/alkemics/CancelablePromise/workflows/End-to-end%20tests/badge.svg?branch=master)](https://github.com/alkemics/CancelablePromise/actions?query=workflow%3A%22End-to-end+tests%22) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/alkemics/CancelablePromise/pulls)

A simple Cancelable Promise.

This package is based on ES Promise.

- See [caniuse](https://caniuse.com/#search=Promise) for browser support
- See [core-js](https://github.com/zloirock/core-js#ecmascript-promise) for polyfills

FYI, you can cancel a fetch request with AbortController & AbortSignal.

- See [MDN](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)
- See [caniuse](https://caniuse.com/#feat=abortcontroller)

## Table of Contents

- [Install](#install)
- [Usage](#usage)
  - [Basic example](#basic-example)
  - [NodeJS](#nodejs)
  - [Browser](#browser)
- [API](#api)
  - [cancelable](#cancelable)
  - [CancelablePromise](#cancelablepromise)
  - [CancelablePromise.cancel](#cancelablepromisecancel)
  - [CancelablePromise.isCanceled](#cancelablepromiseiscanceled)
  - [Static methods](#static-methods)
- [Scripts](#scripts)
  - [Build](#build)
  - [Tests](#tests)
  - [End-to-end tests](#end-to-end-tests)
- [Contributing](#contributing)
  - [Contributors](#contributors)
- [Code of conduct](#code-of-conduct)
- [License](#license)

## Install

```
npm install --save cancelable-promise
```

## Usage

CancelablePromise acts like an ES Promise: you can use `Promise.all`, `Promise.race` with your CancelablePromise for example. The only difference is you'll have a `cancel` method on your promise to cancel future execution of `then` or `catch` functions. CancelablePromise will also cancel all callbacks attached to new promises returned by `then`/`catch`.

**A callback passed to `finally` will be always executed.**

### Basic example

```javascript
import { cancelable, CancelablePromise } from 'cancelable-promise';

const promises = [
  cancelable(new Promise((resolve) => setTimeout(resolve, 1))),
  new CancelablePromise((resolve) => setTimeout(resolve, 1)),
];

for (const promise of promises) {
  promise.then(() => console.log('not logged'));
  promise.cancel();
}
// Nothing will be logged
```

### NodeJS

```javascript
const { cancelable } = require('cancelable-promise');
cancelable(new Promise((resolve) => resolve('ok')));
```

### Browser

```html
<script src="https://unpkg.com/cancelable-promise@3.0.0/dist/CancelablePromise.min.js"></script>
<script>
  const { cancelable } = window.CancelablePromise;
  cancelable(new Promise((resolve) => resolve('ok')));
</script>
```

## API

### cancelable

```javascript
import { cancelable } from 'cancelable-promise';

/**
 * @param {Promise} arg - a native Promise
 * @returns {CancelablePromise}
 */
cancelable(
  new Promise((resolve) => {
    resolve('ok');
  })
);
```

### CancelablePromise

```javascript
import CancelablePromise from 'cancelable-promise';

/**
 * @param {(resolve, reject) => void} arg - a promise executor
 * @returns {CancelablePromise}
 */
new CancelablePromise((resolve, reject) => {
  resolve('ok');
});
```

### CancelablePromise.cancel

```javascript
/**
 * @returns {void}
 */
cancelablePromise.cancel();
```

### CancelablePromise.isCanceled

```javascript
/**
 * @returns {boolean}
 */
cancelablePromise.isCanceled();
```

### Static methods

Same as Promise static methods.

```javascript
import CancelablePromise from 'cancelable-promise';

CancelablePromise.resolve();
CancelablePromise.reject();
CancelablePromise.all([promise1, promise2]);
CancelablePromise.race([promise1, promise2]);
CancelablePromise.allSettled([promise1, promise2]);
```

You can still use the native Promise API and wrap your promise:

```javascript
import { cancelable } from 'cancelable-promise';

cancelable(Promise.all([promise1, promise2]));
cancelable(Promise.race([promise1, promise2]));
cancelable(Promise.allSettled([promise1, promise2]));
```

## Scripts

### Build

Run babel

```
npm run build
```

### Tests

Run `eslint` and `jest`

```shell
npm test
```

### End-to-end tests

Run `cypress`

```shell
npm run cypress
```

## Contributing

Feel free to dive in! [Open an issue](https://github.com/alkemics/CancelablePromise/issues) or [submit PRs](https://github.com/alkemics/CancelablePromise/compare).

### Contributors

This project exists thanks to all [the people who contribute](https://github.com/alkemics/CancelablePromise/graphs/contributors).

## Code of conduct

[Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).

## License

[MIT License](LICENSE) © Alkemics
