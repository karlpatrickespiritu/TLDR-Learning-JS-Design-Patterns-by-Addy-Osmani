### Observer Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript).

### TL;DR

The Observer is a design pattern where an object (known as a subject) maintains a list of objects depending on it (observers), automatically notifying them of any changes to state.

When a subject needs to notify observers about something interesting happening, it broadcasts a notification to the observers (which can include specific data related to the topic of the notification).

When we no longer wish for a particular observer to be notified of changes by the subject they are registered with, the subject can remove them from the list of observers.

### Differences Between The Observer And Publish/Subscribe Pattern

Whilst very similar, there are differences between these patterns worth noting.

- The Observer pattern requires that the observer (or object) wishing to receive topic notifications must subscribe this interest to the object firing the event (the subject).
- The Publish/Subscribe pattern however uses a topic/event channel which sits between the objects wishing to receive notifications (subscribers) and the object firing the event (the publisher).

The minimalist version of Publish/Subscribe I released on GitHub under a project called [pubsubz](http://github.com/addyosmani/pubsubz). This demonstrates the core concepts of subscribe, publish as well as the concept of unsubscribing.

```
var pubsub = {};
 
(function(myObject) {
 
    // Storage for topics that can be broadcast
    // or listened to
    var topics = {};
 
    // An topic identifier
    var subUid = -1;
 
    // Publish or broadcast events of interest
    // with a specific topic name and arguments
    // such as the data to pass along
    myObject.publish = function( topic, args ) {
 
        if ( !topics[topic] ) {
            return false;
        }
 
        var subscribers = topics[topic],
            len = subscribers ? subscribers.length : 0;
 
        while (len--) {
            subscribers[len].func( topic, args );
        }
 
        return this;
    };
 
    // Subscribe to events of interest
    // with a specific topic name and a
    // callback function, to be executed
    // when the topic/event is observed
    myObject.subscribe = function( topic, func ) {
 
        if (!topics[topic]) {
            topics[topic] = [];
        }
 
        var token = ( ++subUid ).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };
 
    // Unsubscribe from a specific
    // topic, based on a tokenized reference
    // to the subscription
    myObject.unsubscribe = function( token ) {
        for ( var m in topics ) {
            if ( topics[m] ) {
                for ( var i = 0, j = topics[m].length; i < j; i++ ) {
                    if ( topics[m][i].token === token ) {
                        topics[m].splice( i, 1 );
                        return token;
                    }
                }
            }
        }
        return this;
    };
}( pubsub ));
```

*Example: Decoupling applications using Ben Alman's Pub/Sub implementation*
```javascript
;(function( $ ) {
 
  // Pre-compile templates and "cache" them using closure
  var
    userTemplate = _.template($( "#userTemplate" ).html()),
    ratingsTemplate = _.template($( "#ratingsTemplate" ).html());
 
  // Subscribe to the new user topic, which adds a user
  // to a list of users who have submitted reviews
  $.subscribe( "/new/user", function( e, data ){
 
    if( data ){
 
      $('#users').append( userTemplate( data ));
 
    }
 
  });
 
  // Subscribe to the new rating topic. This is composed of a title and
  // rating. New ratings are appended to a running list of added user
  // ratings.
  $.subscribe( "/new/rating", function( e, data ){
 
    if( data ){
 
      $( "#ratings" ).append( ratingsTemplate( data ) );
 
    }
 
  });
 
  // Handler for adding a new user
  $("#add").on("click", function( e ) {
 
    e.preventDefault();
 
    var strUser = $("#twitter_handle").val(),
       strMovie = $("#movie_seen").val(),
       strRating = $("#movie_rating").val();
 
    // Inform the application a new user is available
    $.publish( "/new/user", { name: strUser } );
 
    // Inform the app a new rating is available
    $.publish( "/new/rating", { title: strMovie, rating: strRating} );
 
    });
 
})( jQuery );
```

*Example: Decoupling an Ajax-based jQuery application*

```javascript
;(function( $ ) {
 
   // Pre-compile template and "cache" it using closure
   var resultTemplate = _.template($( "#resultTemplate" ).html());
 
   // Subscribe to the new search tags topic
   $.subscribe( "/search/tags", function( e, tags ) {
       $( "#lastQuery" )
                .html("<p>Searched for:<strong>" + tags + "</strong></p>");
   });
 
   // Subscribe to the new results topic
   $.subscribe( "/search/resultSet", function( e, results ){
 
       $( "#searchResults" ).empty().append(resultTemplate( results ));
 
   });
 
   // Submit a search query and publish tags on the /search/tags topic
   $( "#flickrSearch" ).submit( function( e ) {
 
       e.preventDefault();
       var tags = $(this).find( "#query").val();
 
       if ( !tags ){
        return;
       }
 
       $.publish( "/search/tags", [ $.trim(tags) ]);
 
   });
 
 
   // Subscribe to new tags being published and perform
   // a search query using them. Once data has returned
   // publish this data for the rest of the application
   // to consume
 
   $.subscribe("/search/tags", function( e, tags ) {
 
       $.getJSON( "http://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?", {
              tags: tags,
              tagmode: "any",
              format: "json"
            },
 
          function( data ){
 
              if( !data.items.length ) {
                return;
              }
 
              $.publish( "/search/resultSet", { items: data.items } );
       });
 
   });
 
 
})( jQuery );
```

It's one of the easier design patterns to get started with but also one of the most powerful.

### Advantages
- Encourage us to think hard about the relationships between different parts of our application.
- Effectively could be used to break down an application into smaller, more loosely coupled blocks to improve code management and potentials for re-use.
- one of the best tools for designing decoupled systems and should be considered an important tool in any JavaScript developer's utility belt.

### Disadvantages
- In Publish/Subscribe, by decoupling publishers from subscribers, it can sometimes become difficult to obtain guarantees that particular parts of our applications are functioning as we may expect.