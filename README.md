# javascript-patterns-training

### Basics

##### Hoisting

##### This: Scope VS Context
```javascript
function whataScope() {
    return this;
}

console.log(whataScope() === window); // true

var freakingContext = {
        whereAmI: function() {
            return this;
        }
    };

console.log(freakingContext.whereAmI() === freakingContext); // true
```
Adding the **new** keyword, the function uses the brand new created object

##### Closures
```javascript
(function(){

}());
```

##### Namespacing
```javascript
var namespace = namespace || {}

(function(ns){
    ns.MyClass = function() {
        ...
    };
}
}(namespace));

(function(){
    this.MyClass = function() {
        ...
    };
}).apply(namespace));
```


### Classes
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

    }
}

var myClass = new MyClass();
```

##### Shared Functions
```javascript
function MyClass() {}

MyClass.prototype.sharedPublicMethod = function() {

}

var myClass = new MyClass();
```

##### Encapsulation (Private Members)
```javascript
(function(){
    var privateProperty = 'foo',
        privateMethod = function() {

        }
    }

    function MyClass() {}

    MyClass.prototype.getPrivateProperty = function() {
        return privateProperty;
    }

    MyClass.prototype.callPrivateMethod = function() {
        return privateMethod();
        return privateMethod.apply(this, arguments);
    }
}());
```

##### Inheritance / Abstraction
http://stackoverflow.com/a/38086977

```javascript
(function(){
    var privateProperty = 'foo',
        privateMethod = function() {

        }
    }

    function MyClass() {}

    MyClass.prototype.getPrivateProperty = function() {
        return privateProperty;
    }

    MyClass.prototype.callPrivateMethod = function() {
        return privateMethod();
        return privateMethod.apply(this, arguments);
    }
}());
```

