# Writing cleaner JavaScript with ES6+ (How many do you know?)

## Nullish coalescing operator (??)
The nullish coalescing operator is amazing when working with possibly undefined data.\
This operator tells JavasSript to return the data on its right side when its left side is null or undefined (nullish).
```javascript
// returns 'default value'
null ?? 'default value'
```
This operator can be used to define a default value for possibly nullish data, avoiding verbose code checking if some data is not defined.
```javascript
// we pass a default string into our function if 'name' is not defined.
customFunc(name ?? 'default')
```
### Note vs OR operator
A lot of people think this is what the OR (||) operator does.\
However, the OR operator returns its right side when the left side is **falsy**, not just nullish. This includes data such as 0 and ''.

## Optional chaining (?.)
Using optional chaining, when accessing properties, if any property reference is nullish, the expression returns undefined instead of causing an error.
```javascript
const object = {
  property1: {
    name: 'P1'
  }
}

// returns undefined and doesn't cause an error
return object.property2?.name
```
This also works when calling functions.
```javascript
// will call 'customFunc' if it exists on 'object', or return undefined if not.
object.customFunc?.()
```

Optional chaining can be used to avoid having a conditional statment every time there is some data that can be possibly undefined, making your code significantly cleaner.

---
### Tip: The nullish coalescing and optional chaining operators are best used with TypeScript, since you'll know exactly when to use them.
---

## Logical AND Short-circuit Evaluation
When using the AND (&&) operator, the right side expression is only evaluated if the first value is truthy.
```javascript
// returns 'this'
true && 'this'
```
Short circuiting with the && operator allows you to evaluate a condition before you call a function.\
This way, you can avoid the need to write a verbose if statement before calling something. 
```javascript
// calls func() if 'variable' is true
variable && func()
```

## Includes()
Array.includes() is a way of checking if an array contains something.
```javascript
[1, 2, 3].includes(2) // returns true
```
This can be used to as a way of avoiding multiple conditional checks, shortening your code, as well as making it more readable.
```javascript
// instead of this
if(var === 'A' || var === 'B' || var === 'C')
  return var
  
// do this
if(['A', 'B', 'C'].includes(var)) 
  return var
```

## For Of & forEach()
Looping can be done much cleaner using 'for of' and '.forEach()', rather than a traditional for loop.\
A big point for using forEach() is that it can be chained, making your code much more concise and readable.
```javascript
// a tiny amount of code for looping over wanted items in an array.
// can be chained further for more complicated cases.
arr.filter((i) => i > 0).forEach((v, i) => console.log(v));
```
On the downside, there are a lot of edge cases when using forEach(), such as not including empty elements and not working quite right with async/await code.\
For such cases, its best to use 'for of', which is also pretty concise while not having as many edge cases as forEach() or 'for in'. 
```javascript
// for performing the same function as above, not as concise for more complicated cases but more robust.
for (const i of arr.filter((i) => i > 0)) console.log(i)
// or
for (const i of arr) if(i > 0) console.log(i)
```

## Spread Syntax (...)
The spread syntax has multiple uses useful when trying to keep code consice.\
When used with arrays, it can be used to combine two arrays, or insert something in to an array.
```javascript
// combine two arrays, inserting '3' in between the two.
const arr1 = [1, 2]
const arr2 = [4, 5]

[...arr1, 3, ...arr2] // = [1, 2, 3, 4, 5]
```
Similarly, with objects, we can use the spread syntax to clone another object, while also being able to add new properties.
```javascript
// create a new object with the same properties as obj1 and obj2, while also adding another property 'newProperty'.
const obj1 = {property1: 'p1'}
const obj2 = {property2: 'p2'}

const newObj = {...obj1, ...obj2, newProperty: 'newP'}
// newObj = {property1: 'p1', property2: 'p2', newProperty: 'newP'}
```

## Destructuring and the Rest Operator (...)
Destructuring can be used in many contexts to get distinct variables from array values or object properties.\
This is a great way of cleanly getting deeply nested object properties.
```javascript
// getting the 'id' property from obj.
const obj = {id: 1}

const {id} = obj
// id = 1

// or we can have a custom variable name.
const {id: idLabel} = obj
// idLabel = 1
```
Similarly, the rest operator can be used to seperate out properties from an object.\
This is useful for quickly copying an object while removing some properties.
```javascript
// copying obj1, removing the 'unwanted' property.
const obj = {id: 1, unwanted: 2}

const {unwanted, ...newObj} = obj
// newObj = {id: 1}
```

## Bonus: Remove Duplicates from an Array
Want a quick way of removing duplicate primitive elements from an array? Its very easy with a tiny bit of code using 'new Set()'.\
Combining this with other clean code techniques can lead to some very powerful actions with minimal, yet readable, code.
```javascript
// using set with the spread syntax to combine two arrays, removing duplicates (a union).
const arr1 = [1, 2, 3]
const arr2 = [3, 4, 5]

[...new Set([...arr1, ...arr2])] // equals [1, 2, 3, 4, 5] as an array
```
