# Prototype Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#prototypepatternjavascript).

### TL;DR

Creates objects based on a template of an existing object through cloning. We can think of the prototype pattern as being based on prototypal inheritance where we create objects which act as prototypes for other objects.

```javascript
var myCar = {
 
  name: "Ford Escort",
 
  drive: function () {
    console.log( "Weeee. I'm driving!" );
  },
 
  panic: function () {
    console.log( "Wait. How do you stop this thing?" );
  }
 
};
 
// Use Object.create to instantiate a new car
var yourCar = Object.create( myCar );
 
// Now we can see that one is a prototype of the other
console.log( yourCar.name );
```
If we wish to implement the prototype pattern without directly using `Object.create`, we can simulate the pattern as per the above example as follows:
```javascript
var vehiclePrototype = {
 
  init: function ( carModel ) {
    this.model = carModel;
  },
 
  getModel: function () {
    console.log( "The model of this vehicle is.." + this.model);
  }
};
 
 
function vehicle( model ) {
 
  function F() {};
  F.prototype = vehiclePrototype;
 
  var f = new F();
 
  f.init( model );
  return f;
 
}
 
var car = vehicle( "Ford Escort" );
car.getModel();
```

**Note**: This alternative does not allow the user to define read-only properties in the same manner (as the `vehiclePrototype` may be altered if not careful).

### Advantages
- One of the benefits of using the prototype pattern is that we're working with the prototypal strengths JavaScript has to offer natively rather than attempting to imitate features of other languages.
- Easy way to implement inheritance
- Performance boost: when defining a function in an object, they're all created by reference (so all child objects point to the same function) instead of creating their own individual copies.

### Disadvantages
- It is worth noting that prototypal relationships can cause trouble when enumerating properties of objects and (as Crockford recommends) wrapping the contents of the loop in a `hasOwnProperty()` check. 