# ESLint

[ESLint](https://eslint.org/) is a tool to lint Javascript code, and using special rules it can lint React projects.

## How to use it :ok_hand:

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
# Unit testing

## Mocha
A javascript testing framework for both nodejs and the browser that makes testing easy
### How to use it
* Install Mocha via npm 
```
$ npm install mocha --save-dev
```
* As we [pointed out above](#eslint) in order to use mocha with ESLint we must set mocha as true in the env object inside our **.eslintrc.json** file.
## Chai
Mocha allows us to use any assertion library we want. In this case we are using Chai.

### How to use it
* Install Chai via npm 
```
$ npm install chai --save-dev
```
## Enzyme
According to [Enzyme's website](http://airbnb.io/enzyme/):
> Enzyme is a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output. Enzyme's API is meant to be intuitive and flexible by mimicking jQuery's API for DOM manipulation and traversal.

### How to use it
* Install Enzyme via npm 
```
$ npm install enzyme --save-dev
```
* In order to use Enzyme you need to install an adapter as well. The adapter's version depends on which React version you are currently using. Please check this [page](http://airbnb.io/enzyme/docs/installation/) for more information about Enzyme adapters. For instance, we'll install enzyme-adapter-react-15 as if we were using React 15.
```
$ npm install enzyme-adapter-react-15 --save-dev
```
## Sinon
Sinon is a standalone unit testing library for JavaScript. It provides spies, stubs and mocks.
### How to use it
* Install Sinon via npm 
```
$ npm install sinon --save-dev
```
## Karma

