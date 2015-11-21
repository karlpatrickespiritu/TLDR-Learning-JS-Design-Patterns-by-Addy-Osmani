# Mediator Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#mediatorpatternjavascript).

### TL;DR

A Mediator is an object that coordinates interactions (logic and behavior) between multiple objects. It makes decisions on when to call which objects, based on the actions (or inaction) of other objects and input.

A real-world analogy could be a typical airport traffic control system. A tower (Mediator) handles what planes can take off and land because all communications (notifications being listened out for or broadcast) are done from the planes to the control tower, rather than from plane-to-plane. A centralized controller is key to the success of this system and that's really the role a Mediator plays in software design.

### A Simple Mediator

```javascript
var orgChart = {

  addNewEmployee: function(){

    // getEmployeeDetail provides a view that users interact with
    var employeeDetail = this.getEmployeeDetail();

    // when the employee detail is complete, the mediator (the 'orgchart' object)
    // decides what should happen next
    employeeDetail.on("complete", function(employee){

      // set up additional objects that have additional events, which are used
      // by the mediator to do additional things
      var managerSelector = this.selectManager(employee);
      managerSelector.on("save", function(employee){
        employee.save();
      });

    });
  },

  // ...
}
```

A Mediator is an object that handles the workflow between many other objects, aggregating the responsibility of that workflow knowledge into a single object. The result is workflow that is easier to understand and maintain.

### Similarities And Differences (event aggregator and mediator)

The similarities boil down to two primary items: events and third-party objects

 - **Events**: Both the event aggregator and mediator use events. The event aggregator, as a pattern, is designed to deal with events. The mediator, though, only uses them because it’s convenient.
 - **Third-Party Objects**: Both use a third-party object to facilitate things. In the case of an event aggregator, the third party object is there only to facilitate the pass-through of events from an unknown number of sources to an unknown number of handlers. In the case of the mediator, though, the business logic and workflow is aggregated into the mediator itself.

An event aggregator facilitates a “fire and forget” model of communication. The object triggering the event doesn’t care if there are any subscribers. It just fires the event and moves on. A mediator, though, might use events to make decisions, but it is definitely not “fire and forget”. A mediator pays attention to a known set of input or activities so that it can facilitate and coordinate additional behavior with a known set of actors (objects).


### Advantages
- Reduces the communication channels needed between objects or components in a system from many to many to just many to one.
- Adding new publishers and subscribers is relatively easy due to the level of decoupling present.

### Disadvantages
- Biggest downside of using the pattern is that it can introduce a single point of failure. Placing a Mediator between modules can also cause a performance hit as they are always communicating indirectly. Because of the nature of loose coupling, it's difficult to establish how a system might react by only looking at the broadcasts.

At the end of the day, tight coupling causes all kinds of headaches and this is just another alternative solution, but one which can work very well if implemented correctly.