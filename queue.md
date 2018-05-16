# Queue

Queue is a collection of items that follow **FIFO** (First one in First one out). Think a line

## Queue Class 

Lets first go over the methods 

* enqueue: Adds a new Item to the queue 
* dequeue: Removes the first item from the queue
* front: returns the first element from the queue
* isEmpty: returns boolean true if empty, false if not.
* size: returns length of queue 

Lets go ahead and write up the class 

```javascript
    function Queue() {
        let items = [];
        this.enqueue = function(element) {
            items.push(element);
        }

        this.dequeue = function(element) {

            return items.shift();
        }

        this.front = function() {
            return items[0];
        }

        this.isEmpty = function() {
            return items.length  === 0;
        }

        this.size = function() {
            return items.length;
        }
    }
```

## Priority Queue

```javascript
    function PriorityQueue() {
        let items = [];
        // private class
        function QueueElement(element, priority) {
            this.element = element;
            this.priority = priority;
        }

        this.enqueue = function(element, priority) {
            let queueElement = new QueueElement(element, priority);

            let added = false;
            for(let i=0; i<items.length; i++) {
                if (queueElement.priority  < items[i].priority) {
                    // splice --- index, removedNumberOFItems, AddedItem
                    items.splice(i, 0, queueElement );
                    added = true;
                    break;
                }
            }

            // if item was not added yet then add to the end
            if(!added) {
                items.push(queueElement);
            }
        }

    }
```
