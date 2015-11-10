### Revealing Module Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#revealingmodulepatternjavascript).

### TL;DR

This is a pattern where we would simply define all of our functions and variables in the private scope and return an anonymous object with pointers to the private functionality we wished to reveal as public.

```javascript

    var myRevealingModule = (function () {

        var privateVar = "Ben Cherry",
            publicVar = "Hey there!";

        function privateFunction() {
            console.log("Name:" + privateVar);
        }

        function publicSetName(strName) {
            privateVar = strName;
        }

        function publicGetName() {
            privateFunction();
        }


        // Reveal public pointers to
        // private functions and properties

        return {
            setName: publicSetName,
            greeting: publicVar,
            getName: publicGetName
        };

    })();

myRevealingModule.setName("Paul Kinlan");

```