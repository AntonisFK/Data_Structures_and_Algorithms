# Stack

LIFO dog 🐕, **Last In First Out**.  The addition of new items or removal of existing items takes place at the same end. 

```javascript
    function Stack() {
        let items = [];

        this.push = function(element) {
            items.push(element);
        }

        this.pop = function() {
            return items.pop();
        }

        this.clear = function() {
            items = [];
        }

        this.peek = function() {
            return items[items.length - 1];
        }

        this.isEmpty = function() {
            return items.length === 0;
        }
    }
```