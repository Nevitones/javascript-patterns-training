# javascript-patterns-training

### Basics

##### Hoisting
```javascript
    console.log(foo); // undefined
    console.log(woo); // ReferenceError
    var foo = 'bar';
    console.log(foo); // 'bar'
```

##### This: Scope VS Context
```javascript
function whataContext() {
    return this;
}

console.log(whataContext() === window); // true

var freakingContext = {
        whereAmI: function() {
            return this;
        }
    };

console.log(freakingContext.whereAmI() === freakingContext); // true
```
Adding the **new** keyword, the function uses the brand new created object

##### Apply and Call

```javascript
function whataContext() {
    return this;
}

console.log(whataContext() === window); // true

var freakingContext = {
        whereAmI: function() {
            return this;
        }
    };

console.log(whataContext.apply(freakingContext) === freakingContext); // true
```

##### Closures
The function ability of having its own scope.

**IIFE** (**I**mmediately **I**nvoked **F**unction **E**xpression)

**SIAF** (**S**elf **I**nvoking **A**nonymous **F**unction)

**SEAF** (**S**elf-**E**xecuting **A**nonymous **F**unction)

```javascript
(function(){
    var tryMeOutThere = true;
}());

console.log(tryMeOutThere); // ReferenceError
```

##### Namespacing
```javascript
var namespace = namespace || {}

(function(ns){
    ns.MyClass = function() {
        // ...
    };
}(namespace));

(function(){
    this.MyClass = function() {
        // ...
    };
}).apply(namespace);
```

### Some Patterns

##### Module
Emulates the concept of classes in such a way that we're able to include both public/private methods and variables inside a single object.

##### Prototype
Creates objects based on a template of an existing object through cloning.

##### Observer
Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

##### Singleton
Ensures a class has only one instance by restricting its instantiation to a single object.

##### Facade
Provides a unified and higher-level interface to a set of interfaces in a subsystem that makes the subsystem easier to use.

##### Factory
Defines an interface for creating an object where we can specify the type of object we wish to be created.

### Mimicking Classes
##### Constructor
```javascript
function MyClass() {}

var myClass = new MyClass();
```

##### Public Members
```javascript
function MyClass() {
    this.publicProperty = 'foo';
    this.publicMethod = function() {
        return 'I am public!';
    }
}

var myClass = new MyClass();
console.log(myClass.publicProperty); // 'foo'
console.log(myClass.publicMethod()); // 'I am public!'
```

##### Shared Functions
```javascript
function MyClass() {
    this.publicMethod = function() {};
}

MyClass.prototype.sharedPublicMethod = function() {};

var myClass1 = new MyClass(),
    myClass2 = new MyClass();

console.log(myClass1.publicMethod === myClass2.publicMethod); // false
console.log(myClass1.sharedPublicMethod === myClass2.sharedPublicMethod); // true
```

##### Encapsulation (Private Members) :warning:
http://stackoverflow.com/questions/1441212/javascript-instance-functions-versus-prototype-functions#answer-1441692
```javascript
var MyClass = (function(){
    // Private Shared Members
    var privateSharedProperty = 'foo',
        privateSharedMethod = function() {

        };

    function MyClass() {
        // Private Members
        var privateProperty = 'foo',
            privateMethod = function() {
                // What is my context? :warning:
                console.log(this);
            };
    }

    MyClass.prototype.getPrivateSharedProperty = function() {
        return privateSharedProperty;
    }

    MyClass.prototype.callPrivateSharedMethod = function() {
        return privateSharedMethod.apply(this, arguments);
    }

    MyClass.prototype.getPrivateProperty = function() {
        return privateProperty;
    }

    MyClass.prototype.callPrivateMethod = function() {
        return privateMethod.apply(this, arguments);
    }

    return MyClass;
}());

var myClass = new MyClass();
console.log(); :warning:
```

##### Inheritance / Abstraction
```javascript
var MyClass = (function(){
    var privateProperty = 'foo',
        privateMethod = function() {

        };

    function MyClass() {}

    MyClass.prototype.getPrivateProperty = function() {
        return privateProperty;
    }

    MyClass.prototype.callPrivateMethod = function() {
        return privateMethod.apply(this, arguments);
    }
    
    return MyClass;
}());

var MyOtherClass = (function(){
    function MyOtherClass() {
        // Calling base constructor
        MyClass.apply(this, arguments);
    }
    
    :warning: // MyOtherClass.prototype = new MyClass.prototype(); // Why use new?! // Duplicates the constructor call? http://stackoverflow.com/a/4613017
    MyOtherClass.prototype = MyClass.prototype;
    MyOtherClass.prototype.constructor = MyOtherClass;

    return MyOtherClass;
}());

var myOtherClass = new MyOtherClass();
console.log(myOtherClass instanceof MyOtherClass); // true
```
### Misc :warning:
strict equal operator ===
never omit semicolons
jquery (avoid unnecessary queries)
http://jsperf.com/getelementbyid-vs-jquery-id