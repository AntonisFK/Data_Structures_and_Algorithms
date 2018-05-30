# Linked list 

## 

```javascript
    function LinkedList() {
        let Node = function(el) {
            this.element = el;
            this.next = null;
        }

        let head = null;
        let length = null;

    }
```

appending to the end of the list 
```javascript 
    this.append = function(el) {
        const node = Node(el);
        const current;
        if(head == null) {
            head = node;
        } else {

            current = head;
            while(current.next) {
            // traversing to the end
                current = current.next;
            }
            current.next = node;
        }
        length ++;
    }
```

removing at position
```javascript
    this.removeAt = function(position) {
        // check for out-of bounds values 
        if (position > -1 && position < length) {
            let current = head;
            let previous;
            let index = 0;
            if(positon == 0) {
                head = current.next;
            } else {
                while(index++ < position) {
                    previous = current;
                    current = current.next;
                }
                // lin previous with currents nex 
                previous.next = current.next;
            }
            length--;
            return current.element;
        } else {
            return null;
        }
    }
```

inserting at a position 

```javascript 
    this.addAt = function(el, position) {
        if(position > -1 && position < length) {
            const newNode = Node(el);
            let current = head;
            let previous;
            let index = 0;
            if(position == 0) {
                node.next = current;
                head = node;
            } else {
                while(index++ < position) {
                    previous = current;
                    current = current.next;
                }
                node.next = current;
                pervious.next = node;
            }
        }
    }
```

## Doubly Linked List 

```javascript 
    function DoublyLinkedList() {
        let Node = function(el) {
            this.element = el;
            this.prev = null;
            this.next = null;
        }
        let length = 0;
        let head = null;
        let tail = null;
    }
```

removing element from any position 

```javascript 
    this.removeAt = function(position) {
        // first do check 
        if(position > -1 && position < length) {
            let current = head;
            let pervious;
            let index = 0;
            // removing first item
            if(position === 0) {
                head = current.next;
            }
            if(length === 1) {
                tail = null;
            } else {
                head.prev = null;
                // if its at the end
            } else if (position === length-1){
                previous = current;
                current = current.next; 
            } else {
                while(index++ < postion) {
                    previous = current;
                    current = current.next;
                }
                previous.next = current.next;
                current.next.prev = pervious;
            }
        }
        length --;
        return current.element;
    } else {
        return null;
    }
```

insert at
```javascript
    this.insertAt = function(el, position) {
        if(position >=0 && position <= length) {
            let node = new Node(el);
            let pervious;
            let index = 0;
            let current = head;
            if(position === 0) {
                if(!head) {
                    head = node;
                    tail = node;
                } else {
                    node.next = current;
                    current.prev = node;
                    head = node;
                }
            } else if (position === length) {
                current = tail;
                current.next = node;
                node.prev = current;
                tail = node;
            } else {
                while(index++ < position) {
                    previous = current;
                    current = current.next;
                }
                node.next = current;
                previous.next = node;

                current.prev = previous;
            }
            length ++;
            return true
        } else {
            return false;
        }
    }
```