GruntJS
==========================

In order to use _GruntJS_ you need to set up your project folder with:

- a `package.json` file describing the project and its `NodeJS` dependencies
- a `README.md` file describing your project
- may be a `Git` initialization
- may be an `npm install` to resolve dependencies

## Project Configuration

A `package.json` file is meant to describe your project properties to some kind of automations tools ()like `npm` or `GruntJS`).

A `README.md` file is meant to describe your project in a human readable way. It's content should describe what this project does, how to use it, where to find documentation.

```
// package.json
{
  "name" : "ProjectName",
  "version" : "1.0.0",
  "description" : "A short human readable description",
  
  "repository" : {
    "type" : "git",
    "url" : "git://github.com/account/repository"
  },
  
  "devDependencies": {
    "grunt": "~0.4.1"
  }
}
```

## Install Project Dependencies

The most important part for the pourpose of this sheet is the `devDependencies` property.

This object contains a list of `npm` packages needed by the project which are resolved by:

```
npm install
```

Running this _console_ command you teach `npm` to read your `package.json` file and resolve all dependencies for you.

> Dependencies are downloaded into a `node_modules` folder within your _working directory_. You should add it to your `.gitignore` file to **avoid track dependencies** into your repository!

### Grunt and Plugins

Into `devDependencies` you want to list at least `GruntJS` but you also may need a list of **existing `GruntJS` plugins** you want to use into your tasks.

```
"devDependencies" : {
  "grunt" : "~0.4.1",
  "grunt-contrib-uglify": "~0.2.2"
}
```

### Add Dependencies by Console

During the first project setup you may want to add dependencies while you are installing them.

Instead of run `npm install xxx` and then list it into your `package.json` you should use the `--save-dev` option by which you automatize the process of adding dependencies:

```
npm install grunt-contrib-uglify --save-dev
```

- download `grunt-contrib-uglify` node module into `npm_modules` folder
- add `"grunt-contrib-uglify" : "~0.2.4"` ad package dependency


## Run GruntJS

Now you have `GruntJS` installed as a dependency into your `npm_modules/grunt` folder.

To check if it is working try:

```
grunt --help
```

If all works fine and you see `GruntJS` documentation then you should start building your `Gruntfile.js` file which contains all your `GruntJS` stuffs.

```
// Gruntfile.js
module.exports = function(grunt) {
    console.log("Hello World");
}
```

To run that file just run following instruction into your console:

```
grunt
```

> `GruntJS` will look at your project's `Gruntfile.js` and execute it!

