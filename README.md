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

##### Encapsulation (Private Members)
```javascript
var MyClass = (function(){
    // Private Shared Members
    var privateSharedProperty = 'private shared foo',
        privateSharedMethod = function() {
            return '"Static" method accessing a "static" property: ' + privateSharedProperty;
        };

    function MyClass() {
        // Private Members
        var privateProperty = 'private foo',
            privateMethod = function() {
                return 'Instance method accessing a instance property: ' + privateProperty;
            };

        this.setPrivateProperty = function(value) {
            privateProperty = value;
        }
        
        this.getPrivateProperty = function() {
            return privateProperty;
        }

        this.callPrivateMethod = function() {
            return privateMethod.apply(this, arguments);
        }
    }

    MyClass.prototype.setPrivateSharedProperty = function(value) {
        privateSharedProperty = value;
    }
    
    MyClass.prototype.getPrivateSharedProperty = function() {
        return privateSharedProperty;
    }

    MyClass.prototype.callPrivateSharedMethod = function() {
        return privateSharedMethod.apply(this, arguments);
    }

    return MyClass;
}());

var myClass1 = new MyClass(),
    myClass2 = new MyClass();

/* Private Members */
myClass1.setPrivateProperty('private bar');
console.log('myClass1 Private Property:', myClass1.getPrivateProperty()); // 'private bar'
console.log('myClass2 Private Property:', myClass2.getPrivateProperty()); // 'private foo'

console.log('myClass1:', myClass1.callPrivateMethod());
console.log('myClass2:', myClass2.callPrivateMethod());

try {
    console.log('context:', myClass1.privateMethod()); // Error
} catch(err) {
    console.log('Can\'t access a private method!');
}

/* Private Shared Members */
myClass1.setPrivateSharedProperty('private shared bar');
console.log('myClass1 - Private Shared Property:', myClass1.getPrivateSharedProperty()); // 'private shared bar'
console.log('myClass2 - Private Shared Property:', myClass2.getPrivateSharedProperty()); // 'private shared foo'

console.log('myClass1:', myClass1.callPrivateSharedMethod());
console.log('myClass2:', myClass2.callPrivateSharedMethod());

try {
    console.log('context:', myClass1.privateSharedMethod()); // Error
} catch(err) {
    console.log('Can\'t access a private shared method!');
}
```

##### Inheritance / Abstraction
```javascript
var MyClass = (function(){
    function MyClass() {
        var privateProperty = 'foo';
        
        this.getPrivateProperty = function() {
            return privateProperty;
        }
        
        this.setPrivateProperty = function(value) {
            privateProperty = value;
        }
    }
    return MyClass;
}());

var MyOtherClass = (function(){
    function MyOtherClass() {
        // Calls base constructor
        MyClass.apply(this, arguments);
    }

    // MyOtherClass.prototype = MyClass.prototype; // Shares the prototype
    // MyOtherClass.prototype = new MyClass(); // Calls the constructor twice
    MyOtherClass.prototype = Object.create(MyClass.prototype); // Seems good

    MyOtherClass.prototype.constructor = MyOtherClass;

    return MyOtherClass;
}());

var myClass = new MyClass(),
    myOtherClass = new MyOtherClass();

console.log(myOtherClass instanceof MyClass); // true
console.log(myOtherClass instanceof MyOtherClass); // true
console.log(myOtherClass.constructor === MyOtherClass); // true

myClass.setPrivateProperty('bar');
console.log(myClass.getPrivateProperty()); // bar
console.log(myOtherClass.getPrivateProperty()); // foo
```

#### Don't want to instantiate it :warning:

### Misc :warning:
strict equal operator ===
never omit semicolons
jquery (avoid unnecessary queries)
http://jsperf.com/getelementbyid-vs-jquery-id
be careful with browser support (e.g. forEach)


### References
http://stackoverflow.com/questions/1441212/javascript-instance-functions-versus-prototype-functions#answer-1441692
http://stackoverflow.com/a/4613017
https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#Example