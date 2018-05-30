# Dictionaries and Hasehs 

## Dictionary

A **dictionary** is used to store *[key, value]* pairs, where the key is used to find a particular element. A dictionary is also known as a map. 

```javascript 
    function Dictionary() {
        var items = {};

        this.has = function(key) {
            return key in items;
        }

        this.set = function(key, value) {

            items[key] = value;
        }

        this.delete = function(key) {
            if(this.has(key)) {
                delete items[key];
                return true;
            }

            return false;
        }

        this.get = function(key) {
            return this.has(key) ? items[key] : undefined;
        }

        this.values = function() {
            var values = [];
            for (var k in items) {

                if(this.has(k)) {
                    values.push(items[k])
                }
            }

            return values;
        }

    }
```

## Hash table

Lets learn about HashTable class also known as HashMap, a has implementation of the Dictionary class.
**Hashing** consist of finding a value in a data structure in the shortest time possible. 

The `loseloseHashMethod` takes a key parameter and will generat a number-based on the sum of each char ASCII value that composes key.  On this line `hash % 37` we use rest of the division (mod) of the hash number using an arbitrary number.

For the HashTableClass we dont need to remove the position of the table like in a Array. Because we are inserting elements to random indexes there will be a bunch of elements in the array that wil already have undefined elements

```javascript
    function HashTable() {
        var table = [];
        // private hash method
        var loseloseHashCode = function(key) {
            var hash = 0;
            for (var i = 0; i < key.length; i++) {
                hash += key.charCodeAt(i);
            }
            return hash % 37;
        }

        this.put = function(key, value) {
            var position = loseloseHashCode(key);
            console.log(`${position} - ${key}`);

            table[position] = value;
        }

        this.remove = function(key) {
            table[loseloseHashCode(key)] = undefined;
        }
    }
```

### Handling collisions between hash tables

Sometime different keys will have the same hash value. We will call it a collision as we will try to set different values to the same position of the HashTable instance. There are a few techniques to handle collisions: separate chaining, linear probing, and double hashing.

#### Separate chaining

The **Separate chaining** technique consists of creating a linked list for each position of the table and storing the elements in it. It is the simplest technique to handling collisions however, it requires additionally memory outside the hashTable instance.

We will need a new helpler class to represent the element we will add to the LinkedList instance.

```javascript
    var ValuePair = function(key, value) {
        this.key = key;
        this.value = value;

        this.toString = function() {
            return `[${this.key} - ${this.value}]`
        }
    }
```

Lets Update the Put method 

```javascript 
    this.put = function(key, value) {
        var position = loseloseHashCode(key);

        if(table[position] === undefined ) {
            table[position] = new LinkedList();
        }
        // addd to linked List
        table[position].append(new ValuePair(key, value))
    }
```

Now the Get method

```javascript
    this.get = function(key) {
        var position = loseloseHashCode(key);

        if(table[position] !== undefined ) {
            //iterate linked list to find key value 

            var current = table[position].getHead();

            while(current.next) {
                if(current.element.key === key) {
                    return current.element.value;
                }
                current = current.next;
            }
            // check in case first or last element
            if (current.element.key === key) {
                return current.element.value;
            }
        }
        return undefined;
    }
```

Remove method 

```javascript
    this.remove = function(key) {
        var position = loseloseHashCode(key);

        if(table[position] != undefined) {

            var current = table[position].getHead();
            //Iterate thru the linked list
            while(current.next) {
                if(current.element.key === key) {

                    table[position].remove(current.element);
                    // check if its empty 
                    if(table[position].isEmpty()) {
                        table[position] = undefined;
                    }
                    return true;
                }
                current = current.next;
            }
            // check in case first or last element 
            if (current.element.key === key) {
                table[position].remove(current.element)

                if (table[position].isEmpty()) {
                        table[position] = undefined;
                }
            }

        }
        return false;
    }
```

#### Linear probing 

Another technique of collision resolution is linear probing. When we try to add a new element, if the position index is already occupied, then we will try index + 1. If index +1 occupied, then we will try index + 2 and so on. 

We will still use the valuePair helper class from above.

Put method
```javascript
    this.put = function(key, value) {
        var position = loseloseHashCode(key);

        if (table[position] === undefined ) {
            table[position] = new ValuePair(key, value);
        } else {
            var index = ++position;
        }
    }
```

Get method 
```javascript
    this.get = function(key) {
        var position = loseloseHashCode(key);

        if(table[position] !== undefined) {
            if(table[position].key === key) {
                return table[position].value;

            } else {
                var index = ++position;
                while(table[index] === undefined || table[index].key !== key) {

                    index++;
                    if (table[index].key === key) {

                        return table[index].value;
                    }
                }
            }
        }
        return undefined;
    }
```

The remove method is basically the same with a couple of tweaks.
Remove method
```javascript
    this.remove = function(key) {
        var position = loseloseHashCode(key);

        if(table[position] !== undefined) {
            if(table[position].key === key) {
                table[position] = undefined;

            } else {
                var index = ++position;
                while(table[index] === undefined || table[index].key !== key) {

                    index++;
                    if (table[index].key === key) {

                        table[index] = undefined;
                    }
                }
            }
        }
        return false;
    }
```

### Creating better has functions 

A good hash function is composed of certain factors: time to insert and retrieve an element(performance) and also the probabilty to minimize collisions.

