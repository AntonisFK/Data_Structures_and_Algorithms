# Useful tips

## Enhanced object properties 

ES6 introduced concept called *array destructuring* which is a way of initializing variables at once.

```javascript
let [x, y]  = ['a', 'b'];
```

can be used to swap values at once without the create temp variables.

```javascript

    [x , y] = [y, x];

```

Array could use more than just numbers as a index !!

```javascript
    const b = [];
    b['A'] = 0;
    console.log(b);
    // output = [A: 0]
```

### ES6 Map class

```javascript 
    var myMap = new Map();

    var keyString = 'a string',
        keyObj = {},
        keyFunc = function() {};

    // setting the values
    myMap.set(keyString, "value associated with 'a string'");
    myMap.set(keyObj, 'value associated with keyObj');
    myMap.set(keyFunc, 'value associated with keyFunc');

    myMap.size; // 3

    // getting the values
    myMap.get(keyString);    // "value associated with 'a string'"
    myMap.get(keyObj);       // "value associated with keyObj"
    myMap.get(keyFunc);      // "value associated with keyFunc"

    myMap.get('a string');   // "value associated with 'a string'"
                            // because keyString === 'a string'
    myMap.get({});           // undefined, because keyObj !== {}
    myMap.get(function() {}) // undefined, because keyFunc !== function () {}
```

### Closure

A **closure** is an inner function that has access to the outer (enclosing) function's variables-scope chain.
The closure has three scope chains

1. access to its own scope (variables defined inside its curly brackets)
2. it has access to the outer function's variables
3. access to the global variables

Inner functions still have access to outer function's variables even after the outer function has returned. So this means even after the outer function has returned, the inner function has access to the outer function's variables.

```javascript
    function setName(firstName) {

        function setLastName(lastName) {
            return console.log(firstName, lastName)
        }

        return setLastName
    }

    var dude = setName('bob'); // doesnt output anything Its returning the Outer function think about this as setLastName function now

    dude('dole'); // output bob dole

```

Closures could store references to the outer function's variables;

```javascript
    function celebrityID () {
        var celebrityID = 999;
        // We are returning an object with some inner functions​
        // All the inner functions have access to the outer function's variables​
        return {
            getID: function ()  {
                // This inner function will return the UPDATED celebrityID variable​
                // It will return the current value of celebrityID, even after the changeTheID function changes it​
            return celebrityID;
            },
            setID: function (theNewID)  {
                // This inner function will change the outer function's variable anytime​
                celebrityID = theNewID;
            }
        }
    ​
    }
    ​
    ​var mjID = celebrityID (); // At this juncture, the celebrityID outer function has returned.​
    mjID.getID(); // 999​
    mjID.setID(567); // Changes the outer function's variable​
    mjID.getID(); // 567: It returns the updated celebrityId variable 
```

### Fibonacci sequence

```javascript
    function fibonacci(num) {
        if(num === 1 || num === 2) {
            return 1;
        }
        return fibonacci(num - 1) + fibonacci(num -2);
    }
```

non recursive 
``` javascript
    function fib(num) {
        var n1 = 1;
        var n2 = 1;
        var n = 1;
        for(var i=3 ; i<num; i++) {
            n = n1 + n2;
            n1 = n2;
            n2 = n;
        }
        return n;
    }
```

## Web

### Security Issues XSS CSRF

**XSS** also known as cross site scripting is a code injection attack allowing the injection of malicious code into a website. XSS is currently one of the most common website attacks. It can be used to steal users cookies, allowing for someone to pretend to be you on other websites.The javascript can modify the page to make it look and behave differently. If you want to learn more on xss check out this guys [vids](https://www.youtube.com/watch?v=M_nIIcKTxGk).

**CSRF** is an attack that tricks a victim into submiting a malicious request. It inherits the identity and privileges of the victim to perform an undesired function on the victim's behalf. If a user us currently authenticated to a site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim.