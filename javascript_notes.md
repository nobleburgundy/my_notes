# JavaScript Notes File
## Array Pop, Shift, Unshift, Push


## Delete property from collection
```js
delete collection[id].artists;
```

## Recursive function example
Multiply the first n elements of an array to create the product of those elements.
However, notice that ```multiply(arr, n) == multiply(arr, n - 1) * arr[n - 1]```. Example...
```
var testA = [1, 3, 5];
multiply(testA, 3) == multply(testA, 2) * testA[2]
15                 == 1 * 3 * 5
```
That means you can rewrite multiply in terms of itself and never need to use a loop.
```js
  function multiply(arr, n) {
    if (n <= 0) {
      return 1;
    } else {
      return multiply(arr, n - 1) * arr[n - 1];
    }
  }
```
