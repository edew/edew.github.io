---
title: "Hamming weight"
date: "2020-03-15"
description: "How can we calculate the number of 1 bits in a number?"
draft: false
---

Suppose we have the number 45<sub>10</sub>, or 00101101<sub>2</sub>.

How can we calculate the number of 1 bits in the binary representation of the number?

## The algorithm

Luckily this is a well defined problem, in fact Peter Wegner described a algorithm for doing just this sixty years ago!

The function takes a `number` as it's argument and returns the count of 1 bits in that number.

```javascript
const weight = number => {
  let count = 0

  while (number > 0) {
    number &= number - 1
    count += 1
  }

  return count
}
```

While the `number` is greater than zero we reassign it's value to be the bitwise and of `number` and `number - 1` and increment `count` by one. Finally once the number is zero we return the count. This algorithm works by changing the rightmost 1 bit to 0 in each iteration, continuing until all the bits have been set to 0 (i.e. `number` is zero).

Let's walk through the algorithm for the number 9<sub>10</sub>, or 00001001<sub>2</sub>. First we initialise `number` to `9`, `count` to `0`:

```javascript
let number = 9
let count = 0
```

Now we begin. `number` is greater than zero, so we reassign it to `9 & 8` (`00001001 & 00001000`) and increase `count` by one.

```javascript
number = number &= number - 1
count += 1
```

Is `number` still greater than zero? Yes. So we reassign it to `8 & 7` (`00001000 & 00000111`) and increase `count` by one.

```javascript
number = number &= number - 1
count += 1
```

Is `number` still greater than zero? No, so we're done! The number of 1 bits in the original value of `number` is given by the value of `count`, which is `2` in this case.

## Further examples

### Example 1

How many 1 bits are in the binary representation of 1<sub>10</sub>, or 00000001<sub>2</sub>?

| iteration | number   | number - 1 | number & (number - 1) |
| --------- | -------- | ---------- | --------------------- |
| 1         | 00000111 | 00000110   | 00000110              |
| 2         | 00000110 | 00000101   | 00000100              |
| 3         | 00000100 | 00000011   | 00000000              |

After 3 iterations `number` has been reduced to zero, so the count is three.

### Example 2

How many 1 bits are in the binary representation of 7<sub>10</sub>, or 00000111<sub>2</sub>?

| iteration | number   | number - 1 | number & (number - 1) |
| --------- | -------- | ---------- | --------------------- |
| 1         | 00010000 | 00001111   | 00000000              |

After 1 iteration `number` has been reduced to zero, so the count is one.

### Example 3

How many 1 bits are in the binary representation of 31<sub>10</sub>, or 00011111<sub>2</sub>?

| iteration | number   | number - 1 | number & (number - 1) |
| --------- | -------- | ---------- | --------------------- |
| 1         | 00011111 | 00011110   | 00011110              |
| 2         | 00011110 | 00011101   | 00011100              |
| 3         | 00011100 | 00011011   | 00011000              |
| 4         | 00011000 | 00010111   | 00010000              |
| 5         | 00010000 | 00001111   | 00000000              |

After 5 iterations `number` has been reduced to zero, so the count is five.
