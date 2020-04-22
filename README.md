# Searching

## 1. How many searches?
Given a sorted list `3, 5, 6, 8, 11, 12, 14, 15, 17, 18` and using the recursive binary search algorithm, identify the sequence of numbers that each recursive call will search to try and find 8.
```
[ 3,  5,  6,  8, 11, 12, 14, 15, 17, 18 ]
[ 3, 5, 6, 8, 11 ]
[ 6, 8, 11 ]
```

Given a sorted list `3, 5, 6, 8, 11, 12, 14, 15, 17, 18` and using the recursive binary search algorithm, identify the sequence of numbers that each recursive call will search to try and find 16.
```
[ 3,  5,  6,  8, 11, 12, 14, 15, 17, 18 ]
[ 12, 14, 15, 17, 18 ]
[ 15, 17, 18 ]
[ 15 ]
```

## 2. Adding a React UI
For exercises 1 and 2, you will be using a search algorithm to search for an item in a dataset. You will be testing the efficiency of 2 search algorithms, linear search and binary search. You will also have a UI (a simple textbox will do) through which you will be sending your input that you want to search. There is no server-side to this program. All of this should be done using React.

1) Linear search

Given the following dataset, find out how many tries it took to search for a particular item in the dataset. If the item is not in the dataset, provide a message and indicate how many searches it took to find that out.

89 30 25 32 72 70 51 42 25 24 53 55 78 50 13 40 48 32 26 2 14 33 45 72 56 44 21 88 27 68 15 62 93 98 73 28 16 46 87 28 65 38 67 16 85 63 23 69 64 91 9 70 81 27 97 82 6 88 3 7 46 13 11 64 76 31 26 38 28 13 17 69 90 1 6 7 64 43 9 73 80 98 46 27 22 87 49 83 6 39 42 51 54 84 34 53 78 40 14 5

2) Binary search

Use the same front end and the dataset from the previous exercise for this exercise. Use array.sort to sort the dataset. Then implement a binary search to find a particular value in the dataset. Display how many tries it took to search for a particular item in the dataset using binary search. If the item is not in the dataset, provide a message and indicate how many searches it took to find that out.

<details><summary>Show Solution</summary>

```js
import React from 'react';
import './App.css';

function App() {

  let binaryCounter = 0;

  function binarySearch(array, query) {
    binaryCounter++;
    const middle = Math.floor(array.length / 2);

    if (array[middle] === query) {
      let result = {
        found: true,
        count: binaryCounter,
      };
      binaryCounter = 0;
      return result;
    }
    else if (array.length === 1) {
      let result = {
        found: false,
        count: binaryCounter,
      }
      binaryCounter = 0;
      return result;
    }
    else if (array[middle] > query) {
      return binarySearch(array.slice(0, middle), query)
    }
    else if (array[middle] < query) {
      return binarySearch(array.slice(middle, array.length), query)
    }
  }

  let linearCounter = 0;

  function linearSearch(array, query) {
    for (let i = 0; i < array.length; i++) {
      linearCounter++;
      if (array[i] === query) {
        let result = {
          found: true,
          count: linearCounter,
        };
        linearCounter = 0;
        return result;
      }
    }
    let result = {
      found: false,
      count: linearCounter,
    }
    linearCounter = 0;
    return result;
  }

  function handleSubmit(e) {
    e.preventDefault();

    const dataset = e.target.dataset.value;
    const query = e.target.query.value;
    const result = document.getElementById('result');

    if (!dataset || !query) {
      result.innerText = 'Please provide a Dataset and Search Term';
      return;
    }

    const binaryCount = binarySearch(dataset.split(' ').sort(), query);
    const linearCount = linearSearch(dataset.split(' '), query);

    if (!binaryCount.found) {
      return result.innerText = `After ${binaryCount.count} binary searches \n and ${linearCount.count} linear searches, \n the search term was not found`;
    }

    return result.innerText = `It took ${binaryCount.count} steps with binary search \n and ${linearCount.count} steps with linear search`;

  }

  const formStyle = {
    'display': 'flex',
    'flexDirection': 'column',
    'width': '300px',
    'margin': '0 auto'
  }

  return (
    <div className="App">
      <h1>Linear vs Binary Search</h1>
      <p>How many steps does each take?</p>

      <form style={formStyle} onSubmit={handleSubmit}>

        <label htmlFor="dataset">Dataset to Search</label>
        <textarea id="dataset"></textarea>

        <label htmlFor="query">Search Term</label>
        <input type="text" id="query"></input>

        <br></br>

        <button>Submit</button>

      </form>

      <h2 id="result">

      </h2>

    </div>
  );
}

export default App;
```

</details>

## 3. Find a book
Imagine you are looking for a book in a library with a Dewey Decimal index. How would you go about it? Can you express this process as a search algorithm? Implement your algorithm to find a book whose Dewey and book title is provided.

```js
// We can use a binary search for the category with the Dewey Decimal number
// Then use a linear search to find the book itself

// Books in a library can be expressed as an array of books within an array of Dewey categories going from 0 to 999

// const categories = [['books'...]...]

// Ignoring the fact that we can just access an array by its index number...

function findCategory(categories, dewey, title) {
  if (dewey < 0 || dewey > 999) {return 'Invalid Dewey Number'}
  if (typeof title !== 'string') {return 'Title must be a string'}

  const middle = Math.floor(categories / 2) 

  if (middle === dewey) {
    return findTitle(categories[middle], title);
  }
  else if (middle < dewey) {
    return findCategory(categories.slice(middle), dewey, title)
  }
  else if (middle > dewey) {
    return findCategory(categories.slice(0, middle), dewey, title)
  }
}

function findTitle(category, title) {
  let result;

  category.forEach(book => {
    if (book === title) {
      result = book;
    }
  })

  return result ? result : 'The book was not found';
}

```

## 4. Searching in a BST
** No coding is needed for these drills**. Once you have answered it, you can then code the tree and implement the traversal to see if your answer is correct.

1) Given a binary search tree whose in-order and pre-order traversals are respectively `14 15 19 25 27 35 79 89 90 91` and `35 25 15 14 19 27 89 79 91 90`. What would be its postorder traversal?
> Answer: 14 15 19 27 25 89 91 90 79 35 

2) The post order traversal of a binary search tree is `5 7 6 9 11 10 8`. What is its pre-order traversal?
> Answer: 8, 6, 5, 7, 10, 9, 11

## 5. Implement different tree traversals
Using your BinarySearchTree class from your previous lesson, create a binary search tree with the following dataset: 25 15 50 10 24 35 70 4 12 18 31 44 66 90 22. Then implement inOrder(), preOrder(), and postOrder() functions. Test your functions with the following datasets.

<details><summary><b>See Solution</b></summary>

```js
  breadthFirstSearch() {
    let currentNode = this.root;
    let list = [];
    let queue = [];
    queue.push(currentNode);

    while (queue.length > 0) {
      currentNode = queue.shift();
      console.log(currentNode.value)
      list.push(currentNode.value);
      if (currentNode.left) {
        queue.push(currentNode.left);
      }
      if (currentNode.right) {
        queue.push(currentNode.right)
      }
    }
    return list;
  }
  breadthFirstSearchR(list = [], queue = []) {
    if (queue.length === 0) {
      return list;
    }
    let currentNode = queue.shift();
    list.push(currentNode.value)
    if (currentNode.left) {
      queue.push(currentNode.left);
    }
    if (currentNode.right) {
      queue.push(currentNode.right)
    }
    return this.breadthFirstSearchR(list, queue)
  }
  dfsInOrder() {
    return traverseInOrder(this.root, [])
  }
  dfsPreOrder() {
    return traversePreOrder(this.root, [])
  }
  dfsPostOrder() {
    return traversePostOrder(this.root, [])
  }
}

function traverseInOrder(node, list) {
  // console.log(node.value)
  if (node.left) {
    traverseInOrder(node.left, list);
  }
  list.push(node.value)
  if (node.right) {
    traverseInOrder(node.right, list)
  }
  return list;
}

function traversePreOrder(node, list) {
  // console.log(node.value)
  list.push(node.value)
  if (node.left) {
    traversePreOrder(node.left, list);
  }
  if (node.right) {
    traversePreOrder(node.right, list)
  }
  return list;
}

function traversePostOrder(node, list) {
  // console.log(node.value)
  if (node.left) {
    traversePostOrder(node.left, list);
  }
  if (node.right) {
    traversePostOrder(node.right, list)
  }
  list.push(node.value)
  return list;
}
```

</details>
<br>
A pre-order traversal should give you the following order: 25, 15, 10, 4, 12, 24, 18, 22, 50, 35, 31, 44, 70, 66, 90
<br>
In-order: 4, 10, 12, 15, 18, 22, 24, 25, 31, 35, 44, 50, 66, 70, 90

Post-order: 4, 12, 10, 22, 18, 24, 15, 31, 44, 35, 66, 90, 70, 50, 25

> Pre Order 25,15,10,4,12,24,18,22,50,35,31,44,70,66,90
In Order 4,10,12,15,18,22,24,25,31,35,44,50,66,70,90
Post Order 4,12,10,22,18,24,15,31,44,35,66,90,70,50,25

## 6. Find the next commanding officer
Suppose you have a tree representing a command structure of the Starship USS Enterprise.

```
               Captain Picard
             /                \
    Commander Riker       Commander Data
      /         \               \
 Lt. Cmdr.   Lt. Cmdr.          Lt. Cmdr.
 Worf        LaForge            Crusher
   /                           /
Lieutenant                  Lieutenant
security-officer            Selar
```

This tree is meant to represent who is in charge of lower-ranking officers. For example, Commander Riker is directly responsible for Worf and LaForge. People of the same rank are at the same level in the tree. However, to distinguish between people of the same rank, those with more experience are on the left and those with less on the right (i.e., experience decreases from left to right). Suppose a fierce battle with an enemy ensues. Write a program that will take this tree of commanding officers and outlines the ranking officers in their ranking order so that if officers start dropping like flies, we know who is the next person to take over command.

```js
// This is essentially just breadth first search. 
// We can return a queue that represents who is in charge as officers dequeue

whosInChargeNext() {
    let currentNode = this.root;
    let list = [];
    let queue = [];
    queue.push(currentNode);

    while (queue.length > 0) {
      currentNode = queue.shift();
      list.push(currentNode.value);
      if (currentNode.left) {
        queue.push(currentNode.left);
      }
      if (currentNode.right) {
        queue.push(currentNode.right)
      }
    }
    return list;
  }
```

## 7. Max profit
The share price for a company over a week's trading is as follows: [128, 97, 121, 123, 98, 97, 105]. If you had to buy shares in the company on a particular day, and sell the shares on a subsequent day, write an algorithm to work out what the maximum profit you could make would be.

```js
// There doesn't seem to be a way to avoid a nested for loop here

function maxProfit(array) {
  // Initialise the max with an actual value from the array
  // We could initialize at 0, but the max profit might be negative
  let max = array[1] - array[0]; // Price sold at minus price bought at
  let days = [array[0], array[1]];

  for (let i = 0; i < array.length; i++) {
    for (let j = i + 1; j < array.length; j++) {
      if (array[j] - array[i] > max) {
        max = array[j] - array[i];
        days[0] = i;
        days[1] = j;
      }
    }
  }

  return `Buy on day ${days[0] + 1} and sell on day ${days[1] + 1} to earn ${max} profit`
}
maxProfit([128, 97, 121, 123, 98, 97, 105])
```

## 8. Egg drop (optional)
This is a fun exercise to do - consider this optional after you are done with all the exercises above. Imagine that you wanted to find the highest floor of a 100 story building that you could drop an egg from without the egg breaking. But you only have 2 eggs. Write an algorithm to find out in the most efficient way which floors you should drop the eggs from. After you have understood the question and made some attempts to solve the problem, go through this reading before you start coding: http://datagenetics.com/blog/july22012/index.html.