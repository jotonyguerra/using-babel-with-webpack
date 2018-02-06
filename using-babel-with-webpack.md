## Learning Objectives

* Learn about ECMAScript
* Understand the purpose of Babel
* Learn how to set up Babel with Webpack


## What is ECMAScript 2015?

ECMAScript is the standardized specification of JavaScript, and this
specification was updated in June 2015 with the ratification of the
ECMAScript 2015 standard (also referred to as ECMAScript 6 or ES6).
ES6 is the first major update to the language since ES5 was standardized in 2009.
[Modern browsers are being updated][browser-updates]
to support ES6 and
[ES.Next][javascript-versioning].

Web browsers are always playing catch-up with the latest JavaScript features.
The only way we can ensure the JavaScript we write works in the largest number of
web browsers is for us to use the old ES5 standard.
But, what if we want to use the fancy new ES.Next features in our code, today?

This can be accomplished by using Babel.


## What is Babel?

[Babel][babel-home] is a JavaScript compiler.
It takes JavaScript code in one format, and transforms it to JavaScript code
which your browser can understand.
This process is known as **transpilation**: the conversion of source code
written in one programming language into another.
Babel is a popular tool because it can transpile between different JavaScript
language specifications (e.g.- ES6 to ES5).
Babel is also commonly used to transpile JSX JavaScript (which is often used in
conjunction with React) to ES5 Javascript.

With this knowledge, let's go setup Babel within our Webpack-based development
environment.


## Following Along

We are going to add Babel to an app which has Webpack set up already.
Run the following commands to get started:

```no-highlight
$ cd ~/challenges
$ et get using-babel-with-webpack
$ cd using-babel-with-webpack
$ npm install
$ npm start
```

## Setting Up Babel With Webpack

Let's start by writing a bit of ES6 in our `main.js` file.

```javascript
let greeting = "hi"
console.log(greeting);
```

The `let` is a new ES6 syntax for variable assignment, and it is currently **not supported** by all browsers.
If we go to [localhost:8080/bundle.js](localhost:8080/bundle.js), and we search for `let greeting` we will find it in our bundle file.
In order to use `let`, we will have to convert it to ES5 syntax.
What does that look like?
There is a tool called [REPL][babel-repl] on the Babel website that allows you to paste in code and see the output of the Babel transpilation on that code.
If we copy and paste the following on the left pane:

```javascript
let greeting = "hi"
```

we should see the following on the right pane:

```javascript
"use strict";

var greeting = "hi";
```

This is the ES5 JavaScript code we want to have in our bundle file instead of the ES6 JavaScript code.
Now that we know what we are aiming for, let's start by adding [`babel-loader`][babel-loader] to our `webpack.config.js`.

```javascript
  module: {
    loaders: [
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: "babel"
      }
    ]
  },
```

The babel loader will work on files whose names end in either `.js` or `jsx`.
It will not be applied to any files that are in a `node_modules` folder.
We also need to install `babel-loader` and its peer dependency `babel-core` via NPM:

```sh
$ npm install --save-dev babel-loader babel-core
```

Out of the box, Babel does not do anything.
To give Babel functionality, we will need to install [Babel Plugins][babel-plugins] to allow Babel to transpile ES6.
In general, one plugin allows Babel to transpile one new feature of ES6.
However, we do not have to specify every single plugin for ES6 because Babel also has a [ES2015 preset][babel-preset-es2015]
This preset is simply a collection of plugins that allows Babel to transpile all ES6 features.
To activate this preset we need to create a `.babelrc` file, which specifies our Babel configuration, in our project's root folder.
The following `.babelrc` sets up Babel to use the ES2015 preset:

```
{
  "presets": ["es2015"]
}
```

We also need to install the ES2015 preset NPM package:

```sh
$ npm install --save-dev babel-preset-es2015
```

If you boot up your Webpack Dev Server and you should see `hi` outputted in console of the Chrome Developers tools.
Furthermore, if you go to [localhost:8080/bundle.js](localhost:8080/bundle.js), you will now not find `let greeting` in the file, but you will find the ES5 equivalent `var greeting`.

## Summary

ECMAScript 2015 is the new standard of JavaScript, and it is not yet fully supported in all browsers.
We can use ES6 today, however, by transpiling the ES6 code we write into ES5 code.
This transpilation can be achieved by setting up Babel with Webpack.

### External Resources

* [Babel][babel-home]
* [babel-loader][babel-loader]

[babel-home]: https://babeljs.io/
[babel-loader]: https://www.npmjs.com/package/babel-loader
[babel-plugins]: https://babeljs.io/docs/plugins/
[babel-preset-es2015]: http://babeljs.io/docs/plugins/preset-es2015/
[babel-repl]: https://babeljs.io/repl/
[browser-updates]: http://kangax.github.io/es5-compat-table/es6/
[javascript-versioning]: https://benmccormick.org/2015/09/14/es5-es6-es2016-es-next-whats-going-on-with-javascript-versioning/
