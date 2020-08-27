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
Another example of returning an array of numbers counting up using recursion...
```js
function countup(n) {
  if (n < 1) {
    return [];
  } else {
    const countArray = countup(n - 1);
    countArray.push(n);
    return countArray;
  }
}
```
Another example of a range of numbers
```js
function rangeOfNumbers(startNum, endNum) {
  if (endNum < startNum) {
    return [];
  } else {
    const arr = rangeOfNumbers(startNum, endNum - 1);
    arr.push(endNum);
    return arr;
  }
};
```
## ES6 
### var vs let
```let``` was introduced in ES6 to provide error checking with variable declarations. ```var``` can be overwritten anytime without any complaints. ```let``` will throw an error (usually in console) if you try to overwrite it. 

Also, the scope of ```var```s are global or local if declared within function, while ```let``` scope is limited to the block, statement, or expression that it's declared in.

```const``` is obviously a constant that is readonly. Best practice dictates that ```consts``` are declared in all caps.
```
const DONTCHANGE = 'Cats are great!';
```
_*Note*_: Objects (including arrays and functions) assigned to a variable using const are still mutable. Using the const declaration only prevents reassignment of the variable identifier. 

const declaration alone doesn't really protect your data from mutation. To ensure your data doesn't change, JavaScript provides a function Object.freeze to prevent data mutation.

### Rest Pararmeter with Functions
Rest parameters allow us to create functions that take multiple parameters without specifiying ahead...
```js
function restParams(...params) {
	return "You have apssed " + params.length;
}
