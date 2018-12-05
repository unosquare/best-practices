# Code Linting
## TSLint

[TSLint](https://palantir.github.io/tslint/) is a tool to lint TypeScript with support to JSX. If you are coding React with Javascript see next section, ESLint.

### How to use it

* Install the TSLint package using npm.
```
$ npm install tslint tslint-react --save-dev
```
* Generate a basic configuration file
```
$ tslint --init
```
* Edit the generated file `tslint.json`, you can add the following configuration to enable React support.
```javascript
{
  "extends": [
    "tslint:recommended",
    "tslint-react"
  ],
  "linterOptions": {
    "exclude": [
      "config/**/*.js",
      "node_modules/**/*.ts"
    ]
  },
  "rules": {
    "quotemark": [
      true,
      "single",
      "avoid-escape",
      "avoid-template"
    ],
    "trailing-comma": [
      false,
      {
        "multiline": "always",
        "singleline": "never"
      }
    ],
    "object-literal-sort-keys": false,
    "no-console": false,
    "jsx-no-lambda": false
  }
}
```
* You can add a NPM script to run `tslint`, and also setup a step in your CI configuration to run it.

## ESLint

[ESLint](https://eslint.org/) is a tool to lint Javascript code and using special rules it can lint React projects.

### How to use it :ok_hand:

* Install the ESLint package using npm.
```
$ npm install eslint --save-dev
```
* Create a *.eslintrc.json* file in the your solution's root folder.
* Install the React ESLint package using npm.
```
$ npm install eslint-plugin-react --save-dev
```
* Copy and paste the following code inside your *.eslintrc.json* file
```javascript
{
    "plugins": [
        "react" //imports react rules from a plugin in this case eslint-plugin-react
    ],
     "env": { 
         "browser": true, //adds browser global variables
         "commonjs": true, //adds CommonJS global variables and scoping 
         "es6": true, //enables ES6 syntax
         "mocha": true // adds all of the Mocha testing global variables
     },
     "parserOptions": { //specifies the JavaScript language options we want to support
         "ecmaFeatures": { //Indicates which additional language features we’d like to use
             "jsx": true, //enables jsx
             "spread": true,
             "experimentalObjectRestSpread": true //enables support for the experimental object spread properties
         },
         "sourceType": "module" //"script" (default) or "module" if your code is in ECMAScript modules.
     },
     "rules": {
         "no-const-assign": "warn",
         "no-this-before-super": "warn",
         "no-undef": "warn",
         "no-unreachable": "warn",
         "no-unused-vars": "warn",
         "constructor-super": "warn",
         "valid-typeof": "warn"
     }
 }
```
* You can add a NPM script to run `eslint`, and also setup a step in your CI configuration to run it.

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
