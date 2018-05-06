# Array

Array is one of the simplest data structures.

Adding a element to the beginning of the array

```javascript
    // add 10 to the beginning
    const numbers = [1,2,4,5,7,89,23,234];

    for (let i=numbers.length; i >= 0; i--) {
        numbers[i] = numbers[i-1];
    }
    // set first number
    numbers[0] = 10;
```

removing element from the first position 

```javascript 
    const numbers = [1,2,4,5,7,89,23,234];

    for (let i=0; i < number.length; i++) {
        numbers[i] = numbers[i+1]
    }
```