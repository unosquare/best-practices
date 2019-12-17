# Code Linting
## Eslint

We moved from TsLint to EsLint.

You can check the following document: https://www.robertcooper.me/using-eslint-and-prettier-in-a-typescript-project

# Unit testing

## Enzyme
According to [Enzyme's website](http://airbnb.io/enzyme/):
> Enzyme is a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output. Enzyme's API is meant to be intuitive and flexible by mimicking jQuery's API for DOM manipulation and traversal.

### How to use it
* Install Enzyme via npm 
```
$ npm install enzyme --save-dev
```
* In order to use Enzyme you need to install an adapter as well. The adapter's version depends on which React version you are currently using. Please check this [page](http://airbnb.io/enzyme/docs/installation/) for more information about Enzyme adapters. For instance, in this example, we'll install enzyme-adapter-react-15 as if we were using React 15.
```
$ npm install enzyme-adapter-react-15 --save-dev
```

## Jest

Zero configuration testing platform, [Jest website](https://jestjs.io/)
> Jest is used by Facebook to test all JavaScript code including React applications. One of Jest's philosophies is to provide an integrated "zero-configuration" experience. We observed that when engineers are provided with ready-to-use tools, they end up writing more tests, which in turn results in more stable and healthy code bases.

### How to use it

* Install Jest via npm
```
$ npm install --save-dev jest
```

* And with typescript
```
$ npm install --save-dev ts-jest @types/jest
```

### Configuration

* We usually use a configuration file like:

```js
const enzyme = require("enzyme");
const Adapter = require("./customAdapter");
const jsdom = require('jsdom');
const { JSDOM } = jsdom;

const open = jest.fn();
const jsdomWrapper = new JSDOM('<!doctype html><html><body></body></html>');
const { window } = jsdomWrapper;

var localStorageMock = (function() {
    var store = {};
    return {
      getItem(key) {
        return store[key];
      },
      setItem(key, value) {
        store[key] = value.toString();
      },
      clear() {
        store = {};
      },
      removeItem(key) {
        delete store[key];
      }
    };
  })();

enzyme.configure({ adapter: new Adapter() });

global.window = window;
global.document = window.document;
global.HTMLElement = window.HTMLElement;
global.localStorage = localStorageMock;
global.open = open;

```

* Modify your project's package.json so that the jest section looks something like:

```json
"jest": {
    "setupFiles": [
      "<rootDir>/test/setupTest.js"
    ],
    "moduleFileExtensions": [
      "ts",
      "tsx",
      "js"
    ],
    "transform": {
      "^.+\\.(ts|tsx)$": "<rootDir>/node_modules/ts-jest/preprocessor.js"
    },
    "testMatch": [
      "<rootDir>/test/**/*.spec.(ts|tsx)"
    ],
    "globals": {
      "ts-jest": {
        "tsConfigFile": "tsconfig.base.json"
      }
    }
  }
```
