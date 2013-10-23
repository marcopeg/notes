KnockoutJS
==========================

## Tips & Tricks

### Force an _computed observable_ to render

There is no native way to force a _computed observable_ to render if binded _observables_ do not change.

A tricky way is to add a custom _observable_ (es. touch) who's role is to be changed and to trigger update to _computed observables_ who use it:
    
    // model structure
    var model = {
        name: ko.observable(),
        surname: ko.observable(),
        touch: ko.observable
    };
    
    model.fullname = ko.computed(function() {
        model.touch();
        return model.name() + " " + model.surname();
    });
    
In above code `model.touch()` is part of _computed_ `fullname` but is no used to compose the outcoming value. However changing its value trigger the _observable_ to update:

    model.touch(new Date().getTime());
    
