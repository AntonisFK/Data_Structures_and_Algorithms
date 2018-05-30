# Sets 

A **set** is a collection of items that are unordered and consists of unique elements. 

```javascript
    function Set() {
        let items = {};

        this.has = function(value) {
            return items.hasOwnProperty(value);
        }

        this.add = function(value) {
            if(!this.has(value)) {
                items[value] = value;
                return true;
            }
            return false;
        }

        this.delete = function(value) {
            if (!this.has(value)) {
                delete items[value];
                return true;
            }

            return false;
        }

        this.size = function() {
            return Object.keys(items).length;
        }
        // alternative legacy function 

        this.sizeLegacy = function() {
            let count = 0;
            for(let key in items) {
                if(items.hasOwnProperty(key)){
                    ++count;
                }
                return count;
            }
        }

        this.values = function() {
            const values = [];
            for( let i=0, keys=Object.keys(items); i < keys.length; i++) {
                values.push(items[keys[i]])
            }
            return values;
        }

    }
```

## Set operations 

* **Union**: Given two sets, this returns a new set with elements from both the given sets
* **Intersection**: Given two sets, this returns a new set with the elements that exist in both sets
* **Difference**: Given two sets, this returns a new set with all the elements that exist in the first set and do not exist the second set. 
* **Subset** This confirms whether a given set is a subset of another set 

Union
```javascript
    this.union = function(otherSet) {
        let unionSet = new Set();

        let values = this.values();
        for (let i=0; i<values.length; i++) {
            unionSet.add(values[i]);
        }
        value = otherSet.values();

        for (let i=0; i<values.length; i++) {
            unionSet.add(values[i]);
        }

        return unionSet;
    }
```

Intersection
```javascript
    this.intersection = function(otherSet) {
        let interSet = new Set();
        const values = this.values();

        for(let i=0; i<values.length; i++) {
            if(otherSet.has(values[i])) {
                interSet.add(values[i]);
            }
        }

        return interSet;
    }
```

Difference
```javascript
    this.difference = function(otherSet) {
        let differenceSet = new Set();

        let values = this.values();
        for(let i=0; i<values.length; i++) {
            if(!otherSet.has(values[i])) {
                differenceSet.add(values[i]);
            }
        }

        return differenceSet;
    }
```

Subset

```javascript
    this.subset = function(otherSet) {
        if (this.size() > otherSet.size()) {
            return false;
        } else {
            let values = this.values();
            for(let i=0; i<values.length; i++) {
                if(!otherSet.has(values[i])) {
                    return false;
                }
            }
            return true;
        }
    }
```

## ES6 Set class

In ES6 they introduced the set class. The difference between our set class and ESg is that the value method returns an Iterator instead of the array with values. Another difference is that we developed a size method to return the number of value set stores. The Est set class ha a property named size.

```javascript
    let set = new Set();
    set.add(3)
    console.log(set.values); // outputs @Iterator
    console.log(set.has(1)); // outputs true
    console.log(set.size); // outputs 1
```