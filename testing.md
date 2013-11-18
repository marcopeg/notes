Testing
==========================

## Tools

Testing Frameworks & Utilities:

- [MochaJS](http://visionmedia.github.io/mocha/)
- [ChaiJS](http://chaijs.com/)
- [SinonJS](http://sinonjs.org)

Test Runners & Utilities:

- [KarmaJS](http://karma-runner.github.io/)
- [PhantomJS](http://phantomjs.org)



## KarmaJS




## Startup

    grunt karma
    // open new terminal
    grunt test
    
## SinonJS

Provides tools to test against code behaviors like calls to libraries.

With provided objects it is possible to fake library objects in the way to test only against test subject code behaviors.

### sinon.spy()

```
var spy = sinon.spy()
```

created an empty anonimous spy object.  
it only act like a tracer for calls.

```
var spy = sinon.spy(function() {
  return 'aaa';
});
console.log(spy());
>> "aaa"
```

create an anonimous spy with its internal logic being run when called.

```
var spy = sinon.spy(jQuery, 'ajax');
… use spy …
spy.restore();
```

wrap `jQuery.ajax` with the spy to trace activity then restore it to the original.  
<small>**NOTE:** the original method still working behind the spy!</small>

`sinon.spy()` return a **spy object** with which you can investigate how spied method had been used.

#### called

#### callCount

#### calledOnce

#### calledTwice

#### calledThrice

#### args

Array of each spy call's arguments:

```
[
  [arg1, arg2],  // first call to the spy
  [arg1, arg2],  // second call to the spy
  ...
]
```

#### getCall(`callIdx`)

return the spy oject for the desired call identified by its numeric index.

#### withArgs(`arg1`, `arg2`, …)

return a spy object filtered by given arguments.

#### firstCall

#### secondCall

#### thirdCall

#### lastCall

#### calledWith()

#### alwaysCalledWith()

#### calledWithExactly()

#### alwaysCalledWithExactly()

#### neverCalledWith()

#### calledBefore(`anotherSpy`)

#### calledAfter(`anotherSpy`)

#### calledOn(`Obj`)

**NOTE:** `Obj` is the context given to the running method

#### calledAlwaysOn(`Obj`)

#### threw(`String TypeError`)
