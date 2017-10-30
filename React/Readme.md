# ESLint

[ESLint](https://eslint.org/) is a tool to lint Javascript code, and using special rules it can lint React projects.

## How to use it

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
A javascript testing framework for both nodejs and the browser for making testing easy
### How to use it
* Install Mocha via npm 
```
$ npm install mocha --save-dev
```
* As we pointed out above in order to use mocha with ESLint we must set mocha as true in the env section inside our **.eslintrc.json** file.
## Chai
Mocha allows us to use any assertion library we want. In this case we are using Chai.

### How to use it
* Install Chai via npm 
```
$ npm install chai --save-dev
```


## Enzyme
## Sinon
## Karma

