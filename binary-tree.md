# Binary trees 🌲

A node in a binary tree has two children at most: one left child and one right child. Store values on the left had side and node with greater value on the right hand side.

So if our root node's value is 11. A value of 3 will go on the left 👈. If the value is 30 the node would be placed on the right 👉🏾.

## Binary Search Tree Class

To start our binary search tree first we must make a node class. Key is the value we be using to set the node.

```javascript 

    function BinarySearchTree()  {

        var Node  = function(key) {
            this.key = key;
            this.right = null;
            this.left = null;
        }
        // no nodes connected so lets set it to null
        var root = null;
    }
```

Here is the methods we are going to add to the Binary Search Tree 

* `insert(key): void` This inserts a new key in the tree
* `search(key): boolean` Returns true if exist false if not
* `inOrderTraverse: void` Visits all nodes of a tree using [in-order traverse](https://en.wikipedia.org/wiki/Tree_traversal#In-order)
* `preOrderTraverse: void` Visits all nodes of a tree using [pre-order traverse ]()
* `postOrderTraverse: void` Vist all the nodes of the tree using post-order
* `min(): key` Returns min key
* `max(): key` Returns max key 
* `remove(key): void ` Removes key from the node 


## Adding a Node 

1. First step is you need create a instance of a node
2. Verify that the insertion is a *special case.* Special cases being if statements conditions like if there is no node then it becomes the root.
3. Add a node to a different position than the root. 

```javascript 
    // part of the tree class
    this.insert = function(key) {
        var newNode = new Node(key);
        // talking about on 2
        if(root === null ) {
            root = newNode;
        } else {
            // calling recursive function 
            insertNode(root, newNode);
        }
    }

```

InsertNode function will help us find out where the correct place to insert a new node is.

```javascript

var insertNode = function(node, newNode) {
    // do check
    if (newNode.key < node.key) {
    // insert if empty
        if(node.left === null) {

            node.left = newNode;

        } else {
    // call it again
            insertNode(node.left, newNode)
        }
    // traverse down the right
    } else {

        if(node.right === null) {

            node.right = newNode;

        } else {

            insertNode(node.right, newNode)
        }
    }
}

```

So then ..

```javascript

    var tree = new BinarySearchTree();
    tree.insert(5);
    tree.insert(4);

```

## Tree traversal

The process of visiting all nodes. The code for traversing isn't that hard. It uses recursion which could be confusing to understood(great [vid](https://www.youtube.com/watch?v=ygK0YON10sQ) ). For me that what helped me understood it was to actually visualizing how the stack would look when all these functions were executed.

### In-order Traversal

An *in-order* traversal visits all the nodes of a BST in ascending order. 🤔 Means that it will visit the nodes from smallest to largest.

```javascript 
    this.inOrderTraverse = function(callback) {
        inOrderTraverseNode(root, callback)
    }
```

The `inOrderTraverse` method takes a callback as a param. This function can be used to perform the action we want to execute when the node is visited.


```javascript 
    var inOrderTraverseNode = function(node, callback) {
        if(node !=== null ) {
            // once node.left is null 👆🏽 if statement will stop it
            inOrderTraverseNode(node.left, callback);
            callback(node.key);
            inOrderTraverseNode(node.right, callback)
        }
    }
```

## Pre-order traversal

Pre-order traversal visits the node prior to its descendants. NLR  Check node first then go left then go up a parent node then go right.
[check this vid](https://www.youtube.com/watch?v=p2UDEb4nR1g)
```javascript
// in tree class 

this.preOrderTraverse = function(callback) {
    // call helper function
    preOrderTraverseNode(root, callback )
}
```

Our Helper function

```javascript

var preOrderTraverseNode = function(node, callback) {
    if (node !== null ) {
        //visit node first N
        callback(node.key);
        // GO Left
        preOrderTraverseNode(node.left, callback);
        // Go Right 
        preOrderTraverseNode(node.right, callback);
    }
}

```

## Post-order traversal

*post-order* traversal visits the node after it visits its descendants. Best way for me to think about this is by *LRN* 
So it means go Left Right Check node
[Check this vid](https://www.youtube.com/watch?v=jDiUxCTdPvM)

1. Traverse Left as much as you can go. Once you cant go anymore output number
2. Move up a parent Traverse right. If you can traverse left then output number.
3. If both children have been visited output parent and move up a parent node. Repeat 1,2. 

Key note --- The Root node should be the last one visited.

## Searching for values in a tree

### Min and Max

With a BST seems pretty straight forward just go left as much as you can to find the min. Go right as much as you can to find the max.

```javascript

// tree class

this.min = function() {
    return minNode(root);
}

this.max = function() {
    return maxNode(root)
}

```

Helper function for min

```javascript 

var minNode = function(node) {

    if(node) {
        // while statement will traverse down node 
        // make sure node exists and you can go down left
        while(node && node.left !== null) {
            // this is going down left
            node = node.left;
        }

        return node;
    }
// no node return null
    return null;
}

```

Right is basically gonna be the same thing but going right. DUh

```javascript 

var maxNode = function(node) {

    if(node) {
        // while statement will traverse down node 
        // make sure node exists and you can go down left
        while(node && node.right !== null) {
            // this is going down left
            node = node.right;
        }

        return node;
    }
// no node return null
    return null;
}
```

### Searching for a specific value

```javascript

    this.search = function(key) {
        return searchNode(root, key)
    }
```

```javascript
    var searchNode = function(node, key) {
        // if node doesnt exist 
        if (node === null) {
            return false;
        }

        if (key < node.key) {

            return searchNode(node.left, key);

        } else if ( key > node.key) {

            return searchNode(node.right, key);

        } else {

            // its been found
            return node
        }
    }
```

## Removing a node

This is the most complex method in the tree. 

```javascript
    // in tree class 
    this.remove = function(key) {
    /*
    * root receives the return of the method removeNode
    */
        root = removeNode(root, key);
    }
```

helper function 

```javascript
    var removeNode = function(node, key) {
        // typical if condition for recursion
        if(node === null ) {
            return null;
        }

        if( key < node.key) {

            node.left = removeNode(node.left , key);

            return node;

        } else if (key > node.key) {

            node.right = removeNode(node.right, key);

            return node;

        } else {
        /*
        * Else if key is equal to node.key 
        */


            //case 1 - a leaf node - no child nodes 
            if (node.left === null && node.right === null) {

                node = null;
                return node;
            }

            // case 2 - node with only one child

            if (node.left === null ) {

                node = node.right;
                return node;

            } else if (node.right === null) {

                node = node.left;
                return node;
            }

            // case 3 - node with 2 children 
            var aux = findMinNode(node.right);
            node.key = aux.key;
            node.right = removeNode(node.right, aux.key);
            return node;

        }


    }

// find min node helper

    var findMinNode = function(node) {
        while (node && node.left !== null ) {
            node = node.left
        }

        return node;
    }

```

### Removing a node with two children

1. Once we find the node we want to remove, we need to find the min node from its right hand side edge subtree.
2. Then, updated the value of the node with the key of minimum node from its right-hand side subtree.
3. Now we have two nodes in the tree with the same key, and this is not allowed. What we need to do now is remove the minimum node from the
right subtree as we moved it t the place of the removed node.
4. Finally, we will return the updated node reference to its parent.

* So basically find the min in the right sub-tree.
* copy the value to the deleted node. 
* Delete the duplicate from the right-subtree.

FYI you could also do this with left max

* Find the max in the left sub tree.
* Copy the value to the deleted node.
* Delete the duplicate from the left-subtree.

TODO: AVL tree and how to balance