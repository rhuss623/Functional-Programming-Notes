# Functional Programming
These are my personal notes on functional programming <br>based on freeCodeCamp's "Functional Programming" lesson materials.
### Learn About Functional Programming
Functional programming follows principles wherein
1. Functions are isolated from changes in the program, including global changes.

2. Functions are pure.  An function will have the same output given the same input, no matter what.

3. Functions will have limited effects on the surrounding program.

### Understand Functional Programming Terminology

* <span style=color:red>Callback</span>:
  * A function that is passed into another function to decide how that function will be invoked.
* <span style=color:red>First class</span> function.
  * A function that can be assigned a variable, passed as an argument, or returned by another function.
  * Every JavaScript function is a <span style=color:red>first class</span> function.
* <span style=color:red>Higher order</span> function:
  * The function that is passing a <span style=color:red>callback</span>, either by passing a function as argument or by returning a function.###
* <span style=color:red>Lambda</span>
  * A function that has been passed into or returned from another function is called a <span style=color:red>lambda</span>.

### Understand the Hazards of Using Imperative Code
Imperative programming differs from functional programming in that imperative statements are used in imperative programming whereas declarative statements are used in functional programming.  

* Imperative statements often change the state of the program, e.g. a <span style=color:red>for</span> loop that iterates over the indices of an array.Imperative statements give the computer a set of statements that tell the computer to perform a task.
* Declarative statements are issued in functional programming by passing methods or functions that tell the computer what to do.

### Avoid Mutations and Side Effects Using Functional Programming
A mutation is a change in the code.  This could be a change to an argument, a global variable, or some other part of the program.  The result of a mutation in the program caused by a function is a side effect.  A pure function causes no side effects.

### Refactor Global Variables Out of Functions
Two takeaways:
1. Don't change variables or objects that are outside the scope of a function.
2. Functions should not rely on any global object or variables.

Here's an example of code that breaks these rules:
```JavaScript
var masterList = [1, 2, 3, 4];

function remove (num) {
  if (masterList.indexOf(num) >= 0){
  return masterList.splice(0, 1, num);
  }
};
```
The code above will alter the original array.

The following code solves this issue.
```JavaScript
var masterList =[1, 2, 3, 4];

function remove (arr, num) {
  let newArr = [...arr]; //The global variable is not used directly in the function.
  if (newArr.indexOf(num) >= 0){
  newArr.splice(newArr.indexOf(num), 1);
  return newArr;   
  }
}
console.log(remove(masterList, 1)); //logs [ 2, 3, 4 ]
console.log(masterList); //logs [ 1, 2, 3, 4 ]
```
### Use the map Method to Extract Data from an Array
When using a function to iterate variables of an array, for loops are imperative functions that can affect their input arrays whereas map functions are declarative functions that do not affect the input array.

```JavaScript
var object = {
  "item1": 1,
  "item2": 2,
  "item3": 3
};
let newObj = object.map( (item) => ({"Item A":item["item1"], "Item B":item["item2"]}) );
```
### Implement map on a Prototype
The following implements <span style=color:red>Array.prototype.forEach()</span> to create a new prototype that behaves as the same as <span style=color:red> Array.prototype.map()</span>
```JavaScript
var test = [1, 2, 3, 4];

Array.prototype.myMap = function(callback){
let newArr = [];
  this.forEach(a => newArr.push(callback(a)));
  return newArr;
};

var testMyMap = test.myMap(function(item){
  return item * 10;
});

console.log(testMyMap); // logs [ 10, 20, 30, 40 ]
console.log(test); // logs [ 1, 2, 3, 4 ]
```
### Use the filter Method to Extract Data from an Array
<span style=color:red>Array.prototype.filter()</span> will not affect the input.  This example uses both <span style=color:red>.filter()</span> and <span style=color:red>.map()</span>
```JavaScript
var object = [{
  "item1": "1",
  "item2": "2",
  "item3": "Three"
},
{
  "item1": "4",
  "item2": "5",
  "item3": "Six"
}];

let newObj = object.filter((x) => Number(x["item1"]) < 2)
.map((x) => ({"Ones Only":x["item1"], "Third Item":x["item3"]}));

console.log(newObj);
//logs [ { 'Ones Only': '1', 'Third Item': 'Three' } ]
console.log(object);
//logs [ { item1: '1', item2: '2', item3: 'Three' },
//  { item1: '4', item2: '5', item3: 'Six' } ]
```
### Implement filter on a Prototype
The following implements <span style=color:red>Array.prototype.forEach()</span> to create a new prototype that behaves the same as <span style=color:red>Array.prototype.filter()</span>.
```JavaScript
var s = [1, 2, 3, 4, 5];

Array.prototype.myFilter = function(callback){
  var newArray = [];
  this.forEach((x) => {if(callback(x)===true){newArray.push(x)}});
  return newArray;
};

var new_s = s.myFilter(function(item){
  return item % 2 === 1;
});

console.log(new_s); // logs [ 1, 3, 5 ]
console.log(s); // logs [ 1, 2, 3, 4, 5 ]
```
### Return Part of an Array Using the slice Method
<span style=color:red>.slice()</span> takes a first argument for the index to begin making a copy and a second argument for the index to stop making a copy of an array.
```JavaScript
let sliceArray = (arr, beginSlice, endSlice) => {
return (arr.slice(beginSlice, endSlice));
};

var input = ["Cat", "Dog", "Tiger", "Zebra", "Ant"];

console.log(sliceArray(input, 1, 3)); //logs [ 'Dog', 'Tiger' ]
console.log(input); // logs [ 'Cat', 'Dog', 'Tiger', 'Zebra', 'Ant' ]
```
### remove
<span style=color:red>.splice()</span> mutates inputs by removing elements whereas <span style=color:red>.slice()</span> makes a new variable of a copy of a specified segment of an array.  This function uses <span style=color:red>.slice()</span> to perform the way <span style=color:red>.splice()</span> would, without mutating the input.
```JavaScript
function nonMutatingSplice(cities) {
  return cities.slice(0,3);
}
let inputCities = ["Chicago", "Delhi", "Islamabad", "London", "Berlin"];

console.log(nonMutatingSplice(inputCities));
//logs [ 'Chicago', 'Delhi', 'Islamabad' ]

console.log(inputCities);
// logs [ 'Chicago', 'Delhi', 'Islamabad', 'London', 'Berlin' ]

```
### Combine Two Arrays Using the concat Method
<span style=color:red>.concat()</span> joins elements without mutating inputs.
```JavaScript
function nonMutatingConcat(original, attach) {
  return (original.concat(attach));
}
var first = [1, 2, 3];
var second = [4, 5];

console.log(nonMutatingConcat(first, second)); //logs [ 1, 2, 3, 4, 5 ]
console.log(first); //logs [ 1, 2, 3 ]
console.log(second); //logs [ 4, 5 ]
```

### Add Elements to the End of an Array Using concat Instead of push
<span style=color:red>.push()</span> mutates inputs, <span style=color:red>.concat()</span> doesn't.
```JavaScript
function nonMutatingPush(original, newItem) {
  return original.concat(newItem);
}
var first = [1, 2, 3];
var second = [4, 5];

console.log(nonMutatingPush(first, second));//logs [ 1, 2, 3, 4, 5];
console.log(first); //logs [ 1, 2, 3 ]
console.log(second); //logs [ 4, 5 ]
```
### Use the reduce Method to Analyze Data
The <span style=color:red>.reduce()</span> method accepts two arguments, (accumulator, currentValue).  The accumulator is a stored value that is acted upon within the reduce function and the currentValue is the value that is being cycled through in the dataset.
```javascript
let object = [{
  "item1": 1,
  "item2": 2,
  "item3": "pickMe"
},
{
  "item1": 4,
  "item2": 5,
  "item3": "pickMe"
},
{
  "item1": 7,
  "item2": 8,
  "item3": "Don't Pick Me"
}];

let item1Val = object.filter((x) => (x.item3==="pickMe")).map((x) => (x.item1));
let averageItem1 = item1Val.reduce((acc, val) => acc + val)/item1Val.length;
console.log(item1Val);//logs [ 1, 4 ]
console.log(averageItem1);//logs 2.5
```
### Sort an Array Alphabetically using the sort Method
When ordering elements in alphabetical or numerical order, use <span style=color:red>.sort()</span>.
```javascript
const alphabetical = arr => {
  return arr.sort((a, b) => {return a > b});
};

let nonAlphabet = ["z", "d", "a", "v", "l", "s"];

console.log(alphabetical(nonAlphabet)); //logs [ 'a', 'd', 'l', 's', 'v', 'z' ]
```
### Return a Sorted Array Without Changing the Original Array
The previous example mutates the original array, which breaks a core tenant of functional programming.  This freeCodeCamp challenge requires the use of <span style=color:red>.concat()</span> to avoid mutating the original. e.g.,
```javascript
var globalArray = [5, 6, 3, 2, 9];

function nonMutatingSort(arr) {
 return [].concat(arr).sort((a,b) => {return a > b});
}

console.log(nonMutatingSort(globalArray));//logs [2, 3, 5, 6, 9]
console.log(globalArray);//logs [ 5, 6, 3, 2, 9 ]
```
One could also complete the challenge using the spread operator:
```javascript
var globalArray = [5, 6, 3, 2, 9];

function nonMutatingSort(arr) {
  let newArr = [...arr];
  return (newArr.sort((a,b) => {return a > b}))
}

console.log(nonMutatingSort(globalArray));//logs [2, 3, 5, 6, 9]
console.log(globalArray);//logs [ 5, 6, 3, 2, 9 ]
```
### Split a String into an Array Using the split Method
Split takes the delimiter as an argument and returns an array with characters that were originally separated by the delimiter.
```javascript
function splitify(str) {
  return str.split(/[^\w]/)
}
console.log(splitify("Hello World,I-am code"));
//logs [ 'Hello', 'World', 'I', 'am', 'code' ]
```
### Combine an Array into a String Using the join Method
Join allows you to convert an array into a string with the delimiter as the argument.
```javascript
function sentensify(str) {
  let arr = str.split(/\W/);
  return arr.join(" ");
}
console.log(sentensify("May-the-force-be-with-you"));
```
### Apply Functional Programming to Convert Strings to URL Slugs
This challenge takes a title-cased sentence and makes it fit for string on a url slug.
```javascript
var globalTitle = "Winter Is    Coming";

function urlSlug(title) {
return title.toLowerCase().split(/\W/).filter((x)=>{return x !==""}).join('-');
};

console.log(urlSlug(globalTitle)); //logs winter-is-coming
```
### Use the every Method to Check that Every Element in an Array Meets a Criteria
The <span style=color:red>every</span> method will perform a function on each value of an array.  If every value passes the test function, the method returns true.  If any one of the values does not pass, the method returns false.
```javascript
function checkPositive(arr) {
return arr.every((currentValue) => {
    return currentValue > 0;
  });
}
console.log(checkPositive([1, 2, 3, -4, 5]));//logs false
console.log(checkPositive([1, 2, 3]));//logs true
```
### Use the some Method to Check that Any Elements in an Array Meet a Criteria
The <span style=color:red>some</span> method acts in a similar fashion to the <span style=color:red>every</span> method. But it returns true if any value passes, and false if not.
```javascript
function checkPositive(arr) {
  return arr.some((currentValue) => {
    return currentValue > 0;
  })
}
console.log(checkPositive([1, -2, -3]));//logs true
console.log(checkPositive([-1, -2, -3]));//logs false
```
### Introduction to Currying and Partial Application

<span style=color:red>arity</span> is the number of arguments a function requires. <span style=color:red>Currying</span> a function converts the function to a nested function which each have an <span style=color:red>arity</span> of 1.
```javascript
function add(x) {
  return (y) => {
    return (z) => {
      return x + y + z;
    }
  }
}
add(10)(20)(30);
```
(From freeCodeCamp)
"Similarly, partial application can be described as applying a few arguments to a function at a time and returning another function that is applied to more arguments.

Here's an example (from freeCodeCamp):"
```javascript
//Impartial function
function impartial(x, y, z) {
  return x + y + z;
}
var partialFn = impartial.bind(this, 1, 2);
partialFn(10); // Returns 13
```
