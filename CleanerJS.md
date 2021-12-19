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
Short circuiting with the AND(&&) operator allows you to evaluate a condition before you call a function.
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

## Includes
```javascript
['A', 'B', 'C'].includes('B')
```

## For Of
```javascript
for (const item of array) console.log(item)
```

## Unique primitive array 
```javascript
new Set([...a, ...b])
```
