# npm-intro
Introduction to npm projects and tools, including linters (ESLint) and Lodash.

## Prerequisites
 * VSCode, node and npm installed

## Introduction to npm
npm stands for Node Package Manager. It let's you install packages to your machine, either globally or in an npm project. Global packages are usually tools you want to run like any other application on your machine, we'll cover some of them later. For now, what we're really interested are the npm projects you create to run a website.

An npm project is really just a directory with a file called `package.json` in it. We'll encounter this file soon, and see how it configures your application to run. You can install many other npm packages inside your own project as _dependencies_. npm packages have their own dependencies too, which means that when you add a package it will usually also install all of the other npm packages that it depends upon.

### Setting up an npm project
 1. Create a new directory, and open it up in a terminal/bash/command window
 1. Type `npm init -y`
    * You'll see a new file created, `package.json`. Open it and have a look through. The file contains a JavaScript object (surrounded by brackets: `{ }`) with a number of keys including `name`, `version` and `description`
 1. Delete the file `package.json`
 1. Now run `npm init`, this time without the `-y` flag
    * Before creating the file, npm now asks you some questions. You can press enter on each question to accept the default values.
 1. For now, give these answers:
    * Leave `package name` and `version` as default
    * Type whatever you like into the `description`
    * For `entry point` answer `src/index.js` - because we're going to put our entry point (the start of our JavaScript application) in a directory called `src`. This is fairly common practice, especially on larger projects - all of the JavaScript you write will go in the `src` directory, so that it doesn't get lost in the muddle of configuration and other files in the root directory of the project.
    * Leave the other questions as default except for `name` which you can... put your name in!
 1. Open up the newly generated `package.json` and see how your answers to all of the questions have been stored there.
 1. Let's install a 3rd party package. Type `npm install lodash` and hit enter. It will probably take a little while for npm to install Lodash, with lots and lots of status messages scrolling past in the meantime. Lodash is a "Swiss army knife" type utility, with lots and lots of functions which make it easier to work with Objects, Arrays, Strings and other JavaScript data types.
    * Once Lodash has finished installing, look inside `package.json` again. You will see that `lodash` has been added to your project's `dependencies`. You can also see which version of Lodash you have installed - mine says `4.17.15` but when you do this exercise you will get the latest release version.
    * Lodash has also added _loads_ of files and directories inside `node_modules/` - have a look at them all! Most of these will be dependencies of Lodash.
 1. Have a look at the subdirectories inside that `node_modules/` directory. At the top are a couple of special ones used by npm, `.bin` and `.cache`. Each of the other directories represents one npm package. Have a look inside a few - some may be full of stuff, but some are fairly simple and will look quite a lot like your npm project, with their own `package.json` and `index.js`
 1. Let's talk about package versions. There is a caret (`^`) in front of the version number listed in our `package.json`, which means that when you or anyone else subseqently set up your project then it will pick a version which is the one specified or newer, but not a newer major version. For example, if your version says `^4.17.15` then your app will accept version `4.17.16` or `4.18.0` but won't accept `5.0.0` or greater.
    * You can find out more about the caret and other modifiers for specifying version numbers, as well as the semver (semantic versioning) rules for comparing versions, at https://docs.npmjs.com/files/package.json#dependencies 
    * To update packages to the latest version allowed by `package.json`, type `npm update`. In the example where you currently have `4.17.15`, if three subsequent versions were available, `4.17.6`, `4.18.0` and `5.0.0` then npm would update your project's version to `4.18.0`
 9. Now let's install another type of dependency. Type `npm install --save-dev eslint`
    * Look inside `package.json` and you will see that `eslint` has been stored under a new key called `devDependencies`. Dependency packages are modules which the web application you're building will depend on, whereas dev dependency packages are tools to help you write
    You'll understand the difference between `dependencies` and `devDependencies` better once  we get on to learning Webpack.
    * There will also be even _more_ stuff inside `node_modules`. From time to time you will probably find yourself deleting your `node_modules`, to remove all installed versions of packages, and freshly reinstall them.

### Checking out an npm project
 1. We're done with the directory you've been working in. Checkout the git repo for this section: TODO: insert repo details
 2. Change to the directory you've just installed, and look inside
    * There is a file `src/index.js` - which is the app's `entry point` specified in `package.json`
    * Our `node_modules` directory and the thousands of files and subdirectories in it are not there.
    * There is a `package.json` in there, identical to the one you created in the last exercise (well, perhaps not exactly identical - if there are newer versions of `lodash` or `eslnt` available, then it will have their version numbers)
 3. The `dependencies` and `devDependencies` listed in `package.json` have not yet been installed to your local version of the project. Install them by typing `npm install`
    * After a little while, the packages will have installed. `node_modules` has reappeared, with all its thousands of pesky files back in it.
    * There is also a new file: `package-lock.json`
 4. Have a quick peek inside `package-lock.json`. It's very long, and contains the version number of each package you have installed, and every one of that package's dependencies.
    * Unlike `package.json`, which can specify a range of permissible versions and let you install the latest one of them, `package-lock.json` tells you the _exact_ version number which you've installed.
    * On most big projects, `package-lock.json` will be committed into the git repo. This ensures that every member of the team will be working with exactly the same set of dependencies and devDependencies, and also tracking the git history of `package-lock.json` helps you to do detective work if you upgrade a dependency and something stops working.

## Introduction to linters
A _linter_ is a tool, distributed as an npm package, that checks your source code to find programming errors, stylistic errors, bugs and suspicious code. It can warn you of potential errors before you upload your code, or even while you're still writing it.

There are several linters available for JavaScript, and ESLint is the most popular. We will learn to install and use it, and to configure it for our own preferences.

### Setting up ESLint
 
 1. We've already got ESLint installed, but ESLint allows for plugins, and we'll need some of these to allow us to select preset versions of JavaScript and style preferences that we want the ESLint rules to enforce. So let's install these plugins as additional dev dependencies: `npm install --save-dev eslint-config-standard eslint-plugin-import eslint-plugin-node eslint-plugin-promise eslint-plugin-standard`
 2. To create a basic configuration for linting, create a file called `.eslintrc.js` (don't forget the dot at the start!)
 3. Inside that file, add: `module.exports = { "extends": "standard" };`
 4. In VSCode, click on the _Extensions_ icon on the left (or hit `Ctrl+Shift+X`), search for _ESLint_, and install the ESLint plugin (it should be the first one in the list of results)

### Finding errors with ESLint
 1. Open `src/index.js`
 2. Add two empty lines to the file
    * If you have ESLint and the VSCode plugin working correctly, you should see a wiggly red line
 3. Hover over the wiggly red line, and a message will pop up: *Too many blank lines at the beginning of file. Max of 1 allowed.eslint(no-multiple-empty-lines)*
 4. Delete all of the new lines, and replace them with the following JavaScript: `var name = 'bob'`
 5. You should now see two wiggly red lines. Hover over the one under `name` and you will see _'name' is assigned a value but never used.eslint(no-unused-vars)_ 
 6. Hover over the other wiggly line and you will see _Newline required at end of file but not found. eslint(eol-last)_
 7. At the end of the line is a light-blue lightbulb. Click on it.
 8. You will see several options. Click the top one, _Fix this eol-last problem_
    * ESLint will fix the problem for you by adding an extra line at the end of the file

### Configuring ESLint rules
ESLint is configured using the file `eslint.rc.js`. You can use existing rule sets, add your own rules and exceptions, and combine multiple sets of rules. At the moment, our `eslint.rc.js` just does one thing: extend (i.e. inherit) the standard set of rules. Let's add some exceptions...
1. Replace the contents of `index.js` with this code: `var first_name = 'bob'`
2. Hover over the wiggly line under `first_name` and you will see two errors. The previous one *'first_name' is assigned a value but never used.eslint(no-unused-vars)* as well as a new error *Identifier 'first_name' is not in camel case.eslint(camelcase)*
3. Add a new line add the end of the file: `console.log(first_name)`
4. Hover over the wiggly line again and you'll see there's now only one error. At the end of the error description it says *eslint(camelcase)*. This - _camelcase_ is the name of the rule that caught this error. Camel Case is the usual way of naming variables in JavaScript, it looks like this: `myVariableName`. But if you want variables named like this: `my_variable_name` then you will have to turn this rule off. 
5. Open up `eslint.rc.js` and inside the curly brackets, after `extends: 'standard'` add a comma and then `"rules": { "camelcase": "off" }`. The whole line should read:
```
module.exports = { extends: 'standard', "rules": { "camelcase": "warn" } };
```
6. Looking again at `index.js`, you will see that there are no longer any errors.
7. Change `eslint.rc.js` so that instead of `"camelcase": "off"` it says `"camelcase": "warn"`
8. Return to `index.js` and you'll see that the wiggly line is back, but this time it's yellow. A warning means something like _this is really not recommended, but if you're *absolutely* sure this is the best way to do it, then go ahead._

The full list of linting rules is at https://eslint.org/docs/rules/  and the full guide for configuring ESLint is at https://eslint.org/docs/user-guide/configuring 

 ### Additional Exercise
  * Replace the contents of `index.js` with some of your own code, typed or pasted in. For example, you could paste your solutions to previous JavaScript exercises. See what potential problems the linter highlights, and how you can either fix them, or turn off the rules that caused them. Before you do this though, try to understand why you might want to use this rule.
  * Investigate the _prettier_ npm package: https://prettier.io/ - Prettier is not a Linter, it doesn't check or fix errors, but it rewrites your code into a standardised format (i.e. it applies rules about newlines, spacing, placing of braces and brackets etc, and formats your code to meet those rules).

## Lodash

### Using Lodash
Needs HTML:
```
<!doctype html>
<html>
<head>
  <title>Getting Started</title>
  -
  <script src="https://unpkg.com/lodash@4.16.6"></script>
</head>
<body>
  -
  <script src="./src/index.js"></script>
</body>
</html>
```

## Revision & Reference
Have a look through the full list of linting rules at https://eslint.org/docs/rules/ - you don't need to learn all of these (!) but it's useful to get an idea of how they're grouped, and what types of things are covered.
You can also disable rules for individual files and blocks of code - see https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments
The full guide for configuring ESLint is at https://eslint.org/docs/user-guide/configuring
