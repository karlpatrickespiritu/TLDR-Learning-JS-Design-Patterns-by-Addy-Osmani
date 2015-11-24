# Flyweight Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#detailflyweight).

### TL;DR

This is a classical structural pattern for optimizing code that is:
  <ul>
    <li>repetitive</li>
    <li>slow</li>
    <li>and inefficiently shares data.</li>
  </ul>
In practice, Flyweight data sharing can involve
  <ul>
  	<li>Taking several similar objects or data constructs used by a number of objects.</li>
  	  <ul>
  	  	<li>
  	  	  AND placing this data into a single external object.
  	  	</li>
  	  </ul>
  </ul>
Usage -- There are two ways in which the pattern can be applied:
  <ul>
    <li>Data layer</li>
    <li>DOM layer: Read <a href="https://davidwalsh.name/event-delegate">David Walsh's</a> article.</li>
  </ul>

Data layer: In flyweight pattern, the data can be of two state:
<ul>
  <li>Intrinsic -- Data values which are absolutely required by methods in an object.</li>
  <li>Extrinsic -- Opposite of intrinic (This is TL;DR)</li>
</ul>

So, Objects with the same intrinsic data can be replaced with a single shared object, created by a factory method. What?... Something like this

```
  function foo() {
    var intrinsicVal = 1;
    this.methodA = function() {
      // use intrinsicVal
	}
  }

  function bar() {
  	var intrinsicVal = 1;
    this.methodB = function() {
      // use intrinsicVal
	}
  }
```
As you can see above, ```intrinsicVal``` has the same value of ```1```. So, flyweight pattern dictates, to do this:

```
  function flyWeight() {
    var intrinsicVal = 1; // Share imlicit values across accessors
    
    // Bungle commonly used methods that access intrinsic values.
    this.methodA = function() {
    // use intrinsicVal
    }
    this.methodB = function() {
      // use intrinsicVal
    }  
  }

  // This just indicates the concept in syntax.
```

Ideal implementation will have:
  <ul>
    <li><b>A flyweight</b>: Defines an interface through which, the implementaion will act on extrinsic states.</li>
    <li><b>Concrete Flyweight</b>: The actual implementation. It should store and manipulate both states</li>
    <li><b>Flyweight Factory</b>: Manage and create flyweight objects. It uses a collection, to identify whether to create new or use return existing obj.</li>
    <li><b>Manager</b>: To manage the extrinsic states.</li>
  </ul>

  // Todo -- A zoo implemntation of the flyweight model

