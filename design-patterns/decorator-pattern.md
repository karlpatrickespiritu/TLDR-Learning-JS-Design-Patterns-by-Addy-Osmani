# Decorator Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#decoratorpatternjavascript).

### TL;DR

Similar to Mixins, they can be considered another viable alternative to object sub-classing. They can be used to modify existing systems where we wish to add additional features to objects without the need to heavily modify the underlying code using them.

The Decorator pattern isn't heavily tied to how objects are created but instead focuses on the problem of extending their functionality. Rather than just relying on prototypal inheritance, we work with a single base object and progressively add decorator objects which provide the additional capabilities. The idea is that rather than sub-classing, we add (decorate) properties or methods to a base object so it's a little more streamlined.

```javascript
// The constructor to decorate
function MacBook() {
 
  this.cost = function () { return 997; };
  this.screenSize = function () { return 11.6; };
 
}
 
// Decorator 1
function memory( macbook ) {
 
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 75;
  };
 
}
 
// Decorator 2
function engraving( macbook ){
 
  var v = macbook.cost();
  macbook.cost = function(){
    return v + 200;
  };
 
}
 
// Decorator 3
function insurance( macbook ){
 
  var v = macbook.cost();
  macbook.cost = function(){
     return v + 250;
  };
 
}
 
var mb = new MacBook();
memory( mb );
engraving( mb );
insurance( mb );
 
// Outputs: 1522
console.log( mb.cost() );
 
// Outputs: 11.6
console.log( mb.screenSize() );
```

In the above example, our Decorators are overriding the `MacBook()` super-class objects `.cost()` function to return the current price of the `Macbook` plus the cost of the upgrade being specified.

It's considered a decoration as the original `Macbook` objects constructor methods which are not overridden (e.g. `screenSize()`) as well as any other properties which we may define as a part of the `Macbook` remain unchanged and intact.

### Advantages
- Fairly flexible. As we've seen, objects can be wrapped or "decorated" with new behavior and then continue to be used without needing to worry about the base object being modified.
- In a broader context, this pattern also avoids us needing to rely on large numbers of subclasses to get the same benefits.

### Disadvantages
-  If poorly managed, it can significantly complicate our application architecture as it introduces many small, but similar objects into our namespace.