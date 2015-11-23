# Mixin Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#mixinpatternjavascript).

### TL;DR

In JavaScript, we can look at inheriting from Mixins as a means of collecting functionality through extension. Mixins allow objects to borrow (or inherit) functionality from them with a minimal amount of complexity. As the pattern works well with JavaScripts object prototypes, it gives us a fairly flexible way to share functionality from not just one Mixin, but effectively many through multiple inheritance.

They can be viewed as objects with attributes and methods that can be easily shared across a number of other object prototypes.

### Example 
```javascript
var myMixins = {
 
  moveUp: function(){
    console.log( "move up" );
  },
 
  moveDown: function(){
    console.log( "move down" );
  },
 
  stop: function(){
    console.log( "stop! in the name of love!" );
  }
 
};
```
We can then easily extend the prototype of existing constructor functions to include this behavior using a helper such as the Underscore.js `_.extend()` method:

```javascript
// A skeleton carAnimator constructor
function CarAnimator(){
  this.moveLeft = function(){
    console.log( "move left" );
  };
}
 
// A skeleton personAnimator constructor
function PersonAnimator(){
  this.moveRandomly = function(){ /*..*/ };
}
 
// Extend both constructors with our Mixin
_.extend( CarAnimator.prototype, myMixins );
_.extend( PersonAnimator.prototype, myMixins );
 
// Create a new instance of carAnimator
var myAnimator = new CarAnimator();
myAnimator.moveLeft();
myAnimator.moveDown();
myAnimator.stop();
 
// Outputs:
// move left
// move down
// stop! in the name of love!
```

### Advantages
- Mixins assist in decreasing functional repetition and increasing function re-use in a system.

### Disadvantages
- Some developers feel that injecting functionality into an object prototype is a bad idea as it leads to both prototype pollution and a level of uncertainty regarding the origin of our functions. In large systems this may well be the case.