NodeJS
======

## require()

`require()` is the way to import a module into a NodeJS script.




### Global Modules VS Local Modules

NodeJS comes with a set of _pre-built modules_ you can use throught `require()` but you can also create your own and use them as well as the global's.

    // import a global module
    var http = require('http');
    
    // import a custom module
    var custom = require('./custom');
    
> The main difference is that **custom modules needs a relative path** to be included.




### The ´node_modules´ Folder

The _global modules_ are provided by NodeJS but you can also create some kind of _custom global modules_ by putting them into a special folder recognised by NodeJS: the `node_modules` folder.

> The `node_modules/` folder should contain you own version of global modules!  
> <small>(or dependencies provided by `NPM`, or your own _project-wide_ libraries)</small>

When you require a global library NodeJS search it inside a `node_modules` beside the file which does the request. If not found it bubble up to the root of the project falling back to NodeJS's global objects.




### What does `require()` provide?

NodeJS `require()` implements the [CommonJS module specification](http://wiki.commonjs.org/wiki/Modules) and **return a reference to requested module**.

    var moduleReference = require('./my-lib');
    var moduleReference = require('global-lib');

- try to identify a relative file or a global module
- _uses a local cache to speed up the process_
- **return a reference** to the module

> The very important concept is "**return a reference**".  
> <small>`require()` works in very different way from other languages "`include()` like" functions!</small>




### Module Private Context & Public API(s)

Every NodeJS module is being executed into a private context then some parts of that module should be explicitly pushed out as module API thanks to the `module.exports` property:

    // module.js
    var privateVar = "private value";
    module.exports.publicMethod = function() {
      return privateVar;
    };
    
    // app.js
    var module = require('./module');
    console.log(module.publicMethod());
    
> When you require a module you obtain **a reference to an instance of that module**.  > The main consequence is that modules private properties **acts much like some static properties** in other languages!

    // module.js
    var privateVar = 0;
    module.exports.set = function(val) {
      privateVar = val;
    };
    module.exports.get = function() {
      return privateVar;
    };
    
    // app.js
    var moduleInstance1 = require('./module');
    moduleInstance1.set(10);
    moduleInstance1.get(); // -> 10
    
    var moduleInstance2 = require('./module');
    moduleInstance2.get(); // -> 10
    moduleInstance2.set(20);
    
    moduleInstance1.get(); // -> 20

Important concepts from above code are following:

#### 1. `require()` returns an instance

_moduleInstance1_ and _moduleInstance2_ **are the same**!

    moduleInstance1 === moduleInstance2
    -> true

#### 2. private variables acts like static

The first time you require a library a fresh instance of that library being created and **stored into a local cache** so other requests fetches that instance from that cache.

> Module's code is not evaluated more than once!

**NOTE:** this behavior is a good behavior for performances but could cause big problems if you don't care to understand what modules are and how NodeJS deals with them! 

#### 3. modules are not classes!

NodeJS modules are not meant to be just some "class definition" files and the `require()` function is very different from other languages `include()` utility!



### Use a Module as a Class Definition File

> **following code only show "a way" you can use NodeJS modules but this not mean you should really use it!**

In _browser JavaScript_ or in other languages you was in the habit of define a class into a file, include that file, and then create new instances of that class  
<small>(just think on [AMD modules](http://requirejs.org/docs/whyamd.html)).</small>

With NodeJS you could reproduce this behavior as follow:

    // module.js
	module.exports = function() {
		var privateVar = 0;
		this.set = function(val) {
		  privateVar = val;
		};
		this.get = function() {
		  console.log(privateVar);
		}
	}
	
	// app.js
	var moduleClass = require('./module');
	var moduleInstance = new moduleClass();

In above code you **export a _function_ as whole module value** so the `require()` instruction acts only like a _pass throught_ to that function!

Although you can do this, I don't think is the best way to solve the problem of creating different instances of a class.

You can define your module and provide it with a public method to create a new instance:

	// module.js	
	var staticVar = 0;
	
	var ClassDef = function() {
  	  var privateVar = 0;
	  this.set = function(val) {
	    staticVar = val;
	    privateVar = val;
	  };
	  this.get = function() {
	    return {
	      "static" : staticVar,
	      "private" : privateVar
	    };
	  };
	  this.log = function() {
		console.log(this.get());  
	  };
	};
	
	module.exports.createInstance = function() {
	  return new ClassDef();
	};

Above module definition contain a **private constructor** `ClassDef` which exposes two methods to access some kind of private variables.

Then the module exposes a single public method `createInstance` which is responsible of the instance generation process.

    // app.js
    var module = require('./module');
    
    var is1 = module.createInstance();
    var is2 = module.createInstance();
    
    console.log("Are the same?", is1 === is2);
    
    is1.set(10);
    is2.set(20);
    
    is1.log();
    is2.log();
    
    // ---> Console Results:
    Are the same? false
    { static: 20, private: 10 }
    { static: 20, private: 20 }

This approach allow your to module to take control the way any instances being created.  
<small>You may want to add some kind of constructor data validator…</small>

> This example also show the side effect of using module's private variables.  
> `staticVar` is shared between all class instances created within the module.  
> `privateVar` being creaded inside the instance so it is a really instance private variable.
