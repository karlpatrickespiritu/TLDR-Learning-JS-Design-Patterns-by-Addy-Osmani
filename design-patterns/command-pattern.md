# Command Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#commandpatternjavascript).

### TL;DR

The Command pattern aims to encapsulate method invocation, requests or operations into a single object and gives us the ability to both parameterize and pass method calls around that can be executed at our discretion. 

It provides us a means to separate the responsibilities of issuing commands from anything executing commands, delegating this responsibility to different objects instead. They consistently include an execution operation (such as run() or execute()).

###Example

```javascript
(function(){
 
  var carManager = {
 
    // request information
    requestInfo: function( model, id ){
      return "The information for " + model + " with ID " + id + " is foobar";
    },
 
    // purchase the car
    buyVehicle: function( model, id ){
      return "You have successfully purchased Item " + id + ", a " + model;
    },
 
    // arrange a viewing
    arrangeViewing: function( model, id ){
      return "You have successfully booked a viewing of " + model + " ( " + id + " ) ";
    }
 
  };
 
})();
```

Here is what we would like to be able to achieve:

```javascript
carManager.execute( "buyVehicle", "Ford Escort", "453543" );
```

As per this structure we should now add a definition for the carManager.execute method as follows:

```javascript
carManager.execute = function ( name ) {
    return carManager[name] && carManager[name].apply( carManager, [].slice.call(arguments, 1) );
};
```

Our final sample calls would thus look as follows:

```javascript
carManager.execute( "arrangeViewing", "Ferrari", "14523" );
carManager.execute( "requestInfo", "Ford Mondeo", "54323" );
carManager.execute( "requestInfo", "Ford Escort", "34232" );
carManager.execute( "buyVehicle", "Ford Escort", "34232" );
```

### Advantages
- it enables us to decouple objects invoking the action from the objects which implement them, giving us a greater degree of overall flexibility in swapping out concrete classes (objects).