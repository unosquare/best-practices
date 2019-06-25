Table of contents
=================
* [React Context](#React-Context)
* [Code Linting](#Code-Linting)
* [Unit testing](#Unit-testing)

# React Context Implementation
We have implemented [React Context](https://reactjs.org/docs/context.html) in a way that allows us to share accross the tree: global state and global logic. Along with that, we also wanted a way [Global snackbar](https://material-ui.com/components/snackbars/) to show any feedback from the app.

During this implementation we have learned several things about react.

## First attempt

Our first approach was creating a simple GlobalContextProvider, exposing some information about the user:
- IsAuthenticated
- Username

And some common actions like:
- login
- logout
- setSnackbarMessage

So, our Context Provider would look like:

```tsx
const GlobalContextProvider: React.FunctionComponent<any> = ({
	children
}) => {
	const [getMessage, setMessage] = React.useState({
		messageText: "",
		messageType: "success"
	});

	const setSnackbarMessage = (text: string, type = "success") =>
		setMessage({
			messageText: text,
			messageType: type
		});

	const [getIsAuthenticated, setIsAuthenticated] = React.useState(false);
	const [getUsername, setUsername] = React.useState("");

	const actions = {
		login: () => {
			console.log("GlobalContextProvider.login");
			setIsAuthenticated(true);
			setUsername("superadmin");

			setSnackbarMessage("User is authenticated");
		},
		logout: () => {
			console.log("GlobalContextProvider.logout");
			setIsAuthenticated(false);
			setUsername("anon");

			setSnackbarMessage("User has been kicked out");
		},
		setSnackbarMessage
	};

	return (
		<GlobalContext.Provider
			value={{
				actions: actions,
				isAuthenticated: getIsAuthenticated,
				username: getUsername
			}}>
			{children}
			<GlobalSnackbar seconds={3000} message={getMessage} mobile={false} />
		</GlobalContext.Provider>
	);
};
```

### Things we learned
One of the things we noticed was the fact that every intent to show a new message in our snackbar was causing a re-render on every component. Please take a look at it: https://codesandbox.io/s/unosquare-best-practices-react-context-1-kqbux

So, we learned that: **Passing `value={{}}` to the provider will re-render any consumer even if the properties inside `value` are the same**

Meaning that we needed to find a way to pass the same `value` if it hasn't changed. That way, consumers won't be updated because there are no changes.

## Second attempt
This can be done by putting that value on a hook.

```tsx
// Instead of doing this
const [getIsAuthenticated, setIsAuthenticated] = React.useState(false);
const [getUsername, setUsername] = React.useState("");
```
```tsx
// We need this
const [getProviderValue, setProviderValue] = React.useState({
  isAuthenticated: false,
  username: '',
  actions: {
    // We will put any actions here
    setSnackbarMessage, // I'm forwarding the setter for our snackbar
  } // We will have another problem here (we will review it later)
});
```

And on the provider:

```tsx
// Instead of doing this
<GlobalContext.Provider
  value={{
    actions: actions,
    isAuthenticated: getIsAuthenticated,
    username: getUsername
  }}>
  {children}
  <GlobalSnackbar seconds={3000} message={getMessage} mobile={false} />
</GlobalContext.Provider>
```
```tsx
// We need this
<GlobalContext.Provider
  value={getProviderValue}>
  {children}
  <GlobalSnackbar seconds={3000} message={getMessage} mobile={false} />
</GlobalContext.Provider>

```

As you can see, we're not passing `getMessage` value to the provider, because that's not relevant to anybody except our GlobalSnackbar.

So, think about this: If any `Consumer` sends a message to the Snackbar, that is going to execute the render on the Provider, but **even though there was a change on the `getMessage` state, the value for the provider has not changed**, which means that no consumer needs to be updated. **We're re-rendering responsibly**.


But now let's see another issue. This is a tricky one. In order to see it, we will add a new action as follows:

```tsx
isValidSession: () => {
  console.log("GlobalContextProvider.isValidSession");

  if (getProviderValue.isAuthenticated) {
    setSnackbarMessage("User is authenticated");
  } else {
    setSnackbarMessage("User is NOT authenticated");

    // Check how getProviderValue won't be changed for
    console.log(
      "Why is not authenticated? getProviderValue: ",
      getProviderValue
    );
  }
}
```

So, let's understand this simple function, we're just checking `getProviderValue.isAuthenticated` and showing a message indicating the result. Try it at: https://codesandbox.io/s/unosquare-best-practices-react-context-2-0xy57

Steps:
1. Open the app
2. Make sure you haven't clicked on any **LOGIN** button
3. Click on Component C -> **Check if user is authenticated on actions**
4. Now, click on Component A -> **LOGIN** button
5. So, now you should be able to see that the user is authenticated
6. Click again on Component C -> **Check if user is authenticated on actions**

What's happening? Consumers using `isAuthenticated` are seeing the proper value but actions on the Provider are seeing a different value.

## Third attempt
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
