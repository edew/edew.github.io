---
title: "Reverse a linked list"
date: "2020-02-04"
description: "How would we write a function to reverse a linked list?"
draft: false
---

Suppose we have a linked list where each node is a Node:

```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}
```

How would we write a function to reverse such a list?

## Iterative

Intuitively we want to "walk" through the list and change each node to point to the previous, stopping when we get to the end.

The iterative reverse function takes the head node of the list as the only parameter and return the new head node once reversed.

Within the function we're going to use three variables to keep track of the previous, current, and next nodes that we want to process. Initially we assign current to be the head node provided as a parameter, and assign previous to be null.

Then while the `current` node is not null we:

- assign `next` to the value of the `current` node's next property
- assign the `current` node's next property to the value of `previous`
- assign `current` to the value of `next`

Finally once the value of current is null we return the value of previous.

```javascript
const reverse = head => {
  let current = head
  let previous = null
  let next

  while (current) {
    next = current.next
    current.next = previous
    previous = current
    current = next
  }

  return previous
}
```

## Recursive

Again, intuitively we want to "walk" through the list and change each node to point to the previous, stopping when we get to the end.

This time instead of using a loop we'll use recursion.

The recursive reverse function takes the current and previous nodes as parameters and return the new head node once reversed.

Let's start with our base case; when `current` is null we want to return the value of `previous`.

If current isn't null then we:

- assign `next` to the value of the `current` node's next property
- assign the `current` node's next property to the value of `previous`
- return the value of recursively calling reverse with `next` as the current parameter, and `current` as the previous parameter

```javascript
const reverse = (current, previous) => {
  if (!current) {
    return previous
  }

  const next = current.next
  current.next = previous

  return reverse(next, current)
}
```

## Example

```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }

  toString() {
    return "[" + this.value + "]->" + this.next
  }
}

const iterative = head => {
  let current = head
  let previous = null
  let next

  while (current) {
    next = current.next
    current.next = previous
    previous = current
    current = next
  }

  return previous
}

const recursive = (current, previous) => {
  if (!current) {
    return previous
  }

  const next = current.next
  current.next = previous

  return recursive(next, current)
}

let head = new Node(1)
head.next = new Node(2)
head.next.next = new Node(3)
head.next.next.next = new Node(4)
head.next.next.next.next = new Node(5)

console.log(head)
// [1]->[2]->[3]->[4]->[5]->null

head = iterative(head)
console.log(head)
// [5]->[4]->[3]->[2]->[1]->null

head = recursive(head, null)
console.log(head)
// [1]->[2]->[3]->[4]->[5]->null
```
