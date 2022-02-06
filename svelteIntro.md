Svelte Tutorial

## Contents
### Intro
### Installation
### 
### 

## Intro
Svelte was rated as the most loved web framework by developers in 2021 (https://insights.stackoverflow.com/survey/2021#technology-most-loved-dreaded-and-wanted). So what is Svelte and why is it so loved?
Svelte is a rather unique javascript framework which aims to be ‘truly reactive’ and help developers ‘write less code’.
It has the awesome feature of creating tiny code bundles by making the framework itself "disappear’ in a compilation stage, shipping optimised vanilla JavaScript code rather than using large and complex libraries loaded at run time. In my opinion, this is the game changer that makes Svelte standout from other JavaScript frameworks. A tiny bundle size means a much faster load time, which seems to be the direction the web is taking as more and more data shows the benefits of a quick website. This compilation stage also removes the need for techniques such as the virtual DOM, used by React and Vue, further increasing the speed of a website.
Another feature is the absence of boilerplate. Svelte feels very close to standard web development, where components can look exactly like vanilla HTML. I’m sure this is a big reason why developers love this framework.
To illustrate this, look at this full svelte component.

As you can see, there is HTML that looks like HTML, CSS that looks like CSS, and JavaScript that looks like JavaScript. This is quite similar to Vue, but without the boilerplate. This lovely, familiar syntax does not require you to learn JSX, or a bunch of new HTML tags. Now let's get into the core concepts of Svelte.

—
Before we start learning how to use Svelte, please share this article and follow me for more quality work.
—

To introduce Svelte, let’s create a simple single page app where users can use a live search bar to filter a list of star wars planets using the [Swapi api](). This will demonstrate all the core features of Svelte in a useful, demonstrable way. Additionally, this is similar to the functionality in my tutorial React app [here](), which I think will nicely illustrate the difference between the two frameworks. The full, completed code for this app will be on my github ([link]()).

Let’s first install basic Svelte. To do this, run npx degit sveltejs/template project-name.
This will copy the svelte starter template into your desired folder.
To enable typescript, go into your new svelte folder, and run node scripts/setupTypeScript.js
Now all you need to do is install the necessary files by running npm install.

—
Component features.
Script - html - style.
A svelte component is a file ending with .svelte. As you can see in App.svelte, a Svelte component can be pretty simple. A svelte file can contain three parts, html, a script tag to put your javascript, and a style tag to place css. This is similar to Vue, just without the boilerplate code.
Clear the script and html contents of App.svelte, and let’s use the given css in the style tag.

Replace App.svelte

Variables + reactivity
Need to store field text + api result items (planets).
We can create a string variable and display it in the DOM very easily.
In the script tag, create a variable like so
```javascript
let name: string = ‘pokemon searcher’
```
This can then be displayed in the app by using curly braces {}.
```javascript
<h1>{name}</h1>
```

For our app, to search through pokemon names, we’ll need an input field, and a variable to store the entered pokemon name.
```javascript
let pokemonName: string = ''
```
To make this variable the contents of the input field, we can use a special svelte keyword ‘bind’, which enables two way binding of the ‘pokemonName’ variable’.
```javascript
<span>Search: </span>
 <input type="text" bind:value="{pokemonName}" />

<h1>Pokemon: {pokemonName}</h1>
```
As you can see, typing into the input field changes the output of the pokemon title.

For this sample app, we will need to store pokemon names from the pokeapi in a variable as an array of strings.
```javascript
let pokemonData: string[] = []
```


Events
Emit event on every character entered into the search field that filters the api result items.

Props
Pass filtered list into DisplayList component.

Ifs and Loops
Display list of planets.

Asynchronous rendering
Render a loading page while waiting for the results from the api.

Stores
Store api result items in store.

