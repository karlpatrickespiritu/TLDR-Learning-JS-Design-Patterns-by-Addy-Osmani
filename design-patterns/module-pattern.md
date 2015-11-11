# Module Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript).

### TL;DR

The Module pattern encapsulates "privacy", state and organization using closures. It provides a way of wrapping a mix of public and private methods and variables, protecting pieces from leaking into the global scope and accidentally colliding with another developer's interface. With this pattern, only a public API is returned, keeping everything else within the closure private.

When working with the Module pattern, we may find it useful to define a simple template that we use for getting started with it. Here's one that covers namespacing, public and private variables:

```javascript

    var myNamespace = (function () {

        var myPrivateVar, myPrivateMethod;

        // A private counter variable
        myPrivateVar = 0;

        // A private function which logs any arguments
        myPrivateMethod = function (foo) {
            console.log(foo);
        };

        return {

            // A public variable
            myPublicVar: "foo",

            // A public function utilizing privates
            myPublicFunction: function (bar) {

                // Increment our private counter
                myPrivateVar++;

                // Call our private method using bar
                myPrivateMethod(bar);

            }
        };

    })();

    console.log(myNamespace)

```

### Advantages
- it supports encapsulation of data
- it's a lot cleaner for developers coming from an object-oriented background than the idea of true encapsulation, at least from a JavaScript perspective.

### Disadvantages
- inability to create automated unit tests for private members and additional complexity when bugs require hot fixes.
