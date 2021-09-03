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

Of course, a real world application utilizes asynchronous actions, something which is rather frustrating in redux. \
In Zustand however, performing asynchronous actions has no additional complexity. Simply tag make the store's funciton as async, and use the await keyword to wait for actions to finish.\
[img]

