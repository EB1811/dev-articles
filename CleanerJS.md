# Writing cleaner JavaScript with ES6+ (How many do you know?)

## Nullish coalescing operator
The nullish coalescing operator is amazing when working with possibly undefined data.\
This operator tells JavasSript to return the data on its right side when its left side is null or undefined (nullish).
```javascript
data1 ?? data2
```
This way, you can pass a default value, and avoid constantly checking if the data is nullish.
```javascript
// returns 'default value'
null ?? 'default value'
```
### Note vs OR operator
A lot of people think this is what the OR || operator does.\
However, the OR operator returns its right side when the left side is FALSY, not just nullish. This includes data such as 0 and ''.

## Optional chaining
```javascript
?.
```
Using optional chaining, when accessing properties, if any property reference is nullish, the expression returns undefined instead of causing an error.
```javascript
const object = {property1: '1'}

// returns undefined and doesn't cause an error
return object?.property2
```
Optional chaining can be used to avoid having a conditional statment every time there is some data that can be possibly undefined, making your code significantly cleaner.

---
### Tip: Nullish coalescing and Optional chaining is best used with TypeScript, since you'll know exactly when to use them.
---

## Logical AND Short-circuit Evaluation
When using the AND (&&) operator, the right side expression is only evaluated if the first value is truthy.
```javascript
// returns 'this'
true && 'this'
```
Short circuiting with the && operator allows you to evaluate a condition before you call a function.
For example, before calling a function, you can check if a variable is true before proceeding.
```javascript
// calls func() if 'variable' is true
variable && func()
```

## Destructuring 
```javascript
{...O, }  {id: idLabel} = O

[...A, ]  [property, ...A]
```

## Rest

## Includes
Array.includes() is a way of checking if an array contains something.\
This can be used to as a way of avoiding multiple if or case statements, shortening your code, as well as making it more readable.
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
For such cases, its best to use 'for of', which is also pretty concise while not having many edge cases as forEach() or 'for in'. 
```javascript
// for performing the same function as above, not as concise for more complicated cases but more robust.
for (const i of arr.filter((i) => i > 0)) console.log(i)
// or
for (const i of arr) if(i > 0) console.log(i)
```

## Unique primitive array
Want a quick way of removing duplicate primitive elements from an array? Its very easy with a tiny bit of code using 'new Set()'.\
Combining this with other clean code techniques can lead to some very powerful actions with minimal, yet readable, code.
```javascript
// using destructuring to combine two arrays, removing duplicates (a union).
const arr1 = [1, 2, 3]
const arr2 = [3, 4, 5]

new Set([...arr1, ...arr2]) // equals [1, 2, 3, 4, 5]
```
