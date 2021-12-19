# Writing cleaner JavaScript with ES6+ (How many do you know?)

### Nullish coalescing operator 
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
#### Note vs OR operator
A lot of people think this is what the OR(||) operator does.\
However, the OR operator returns its right side when the left side is FALSY, not just nullish. This includes data such as 0 and ''.

### Optional chaining 
```javascript
?. ?.()
```

### Logical AND Short-circuit Evaluation
```javascript
true && function()
```

### Destructuring 
```javascript
{...O, }  {id: idLabel} = O

[...A, ]  [property, ...A]
```

### Includes
```javascript
['A', 'B', 'C'].includes('B')
```

### For Of
```javascript
for (const item of array) console.log(item)
```

### Unique primitive array 
```javascript
new Set([...a, ...b])
```
