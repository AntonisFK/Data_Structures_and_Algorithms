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
