# Manage your State the Modern Way – Introduction to Zustand
# Stop overcomplicating your state – Try Zustand with Immer

## Intro
With the advent of hooks a lot of React developers have started to move away from Redux as the default state management of choice. Redux seems to be an amazing tool for a bygone era, with it's opinionated nature becoming a source of frustration rather than joy.
Zustand is a modern state manager that fits nicely in this world of hooks. It is lightweight (only 66.4 kB unpacked), fast, and hooks-based. QUOTE. The brilliance of Zustand is that it’s simple yet powerful.

I stumbled upon Zustand when re-designing our app’s state management. The complexity of Redux and immaturity of React Context made us want to move to another state manager. Having been burned by Redux’s aforementioned complexity, Zustand drew me in with its promised simplicity, describing itself as a ‘barebones’ state manager.

## How to create a store.
First, let's install Zustand.\
[img]\
Creating a store is a very simple process. We use Zustand's 'create' to make a react hook which we will call 'useStore'.\
[img]\
Now we wan't to set the store's initial state.\
We'll create a variable, and a function to update that variable.\
[img]\
And that's it!\
With our store created, let's import it into a React component.\
In this simple example, we will retrieve and display the count variable, and have a button that calls our store's 'incrementCount' function.\
[img]\
As you can see, it's very easy to set up a Zustand store.

### Async Actions
Of course, a real world application utilizes asynchronous actions, something which is rather frustrating in redux. \
In Zustand however, performing asynchronous actions has no additional complexity. Simply tag make the store's funciton as async, and use the await keyword to wait for actions to finish.\
[img]

### Equality Function:
You can define how Zustand checks equality between objects by passing in an equality function as the second parameter. By default, properties are compared with strict-equality, but we can compare using shallow checks by passing in Zustand’s shallow function. The differences between default and shallow are demonstrated below.
You can ofcourse create your own comparison function for greater control over re-rendering.


## Middleware:

Another awesome feature of Zustand is the ability to create middleware to add additional features to your store. For example, you can easily create middleware to log state changes.\
[img]

Redux Dev Tools:
With the middleware functionality, we can easily actually use an amazing extension created for Redux, Redux DevTools [link]. We just need to import the devtools middleware, and attach it to our store.\
[img]\
Now we can visually see everything stored, and look through the store’s timeline, which is pretty cool.

Immer + Typescript.
Immer is another great package that makes reducing nested structures easy. We can create middleware to allow us to use immer easily. Here is a version that preserves property types.\
[img]

## Testing your store:

To test our store using jest, we’ll need some packages. 
React testing library.
Rect testing - hooks.

With react-hooks-testing, it’s very easy to test the functions of our store.\
One important thing to know is that the store’s state is kept between tests. We can deal with this in many ways. One way is to set the content of the store before each test, and another is to set up a mock of Zustand which resets the store each time; you can decide which route to take.\
Now let's test one of our functions:\
[img]\
As you can see, it’s very easy to unit test our store.

In case you are wondering how to test components that use the store, we can easily mock our store with the required returned values.\
[img]

## Conclusion
Having worked with Zustand in a real production environment, I believe that it is quite useful and powerful.
