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

### Variables + reactivity
Variables are created in the script tag. 
We can create a string variable and display it in the DOM very easily.
```javascript
let name: string = ‘pokemon searcher’
```
This can then be displayed in the app by using curly braces {}.
```javascript
<h1>{name}</h1>
```

To search through pokemon names, we’ll need an input field and a variable that holds the contents of that field.
```
<script lang="ts">
let pokemonName: string = ''
</script>
```
To make the ‘pokemonName’ variable equal to the contents of the input field, we can use a special svelte keyword ‘bind’, which enables two way binding of the ‘pokemonName’ variable’.
```
<main>
<span>Search: </span>
 <input type="text" bind:value="{pokemonName}" />

<h1>Pokemon: {pokemonName}</h1>
</main>
```
Now, typing into the input field changes the output of the pokemon title.
This bind keyword enables two way binding without using an ‘onInput’ function that changes the value of the ‘pokemonName’ variable like in React.


### onMount & Async Fetching
For this sample app, we store pokemon names from the pokeapi in a variable as an array of strings.
```javascript
let pokemonData: string[] = []
```
We want to fetch the data and map it as soon as the component is rendered.
For this, we can use ‘onMount’, which is a svelte lifecycle function that runs after the component is first rendered to the DOM. Let’s use this to fetch from the pokeapi and map it into an array of pokemon names.
```javascript
import { onMount } from 'svelte'
```
```javascript
    let pokemonData: string[] = []
    onMount(() => {
        const setPokemonData = async (): Promise<void> => {
            const rawPokemonData = await (
                await fetch('https://pokeapi.co/api/v2/pokemon?limit=99')
            ).json()

            pokemonData = rawPokemonData.results.map(
                (p: { name: string; url: string }) => p.name
            )
        }
        setPokemonData()
      })
```
We now have a list of pokemon names in the ‘pokemonData’ array, which we can use in our simple project.

### Reactive Declarations
For the live search feature, we need to have an array that holds the items filtered by the user input from the pokemon names.

Svelte has an awesome tool to deal with states that are derived from other properties, reactive declarations.
They look like this.
```javascript
$: reactiveVar = otherVar * 2;
```
Now, ‘reactiveVar’ is a variable, but its value is computed every time the ‘otherVar’ variable changes (svelte runs the computation when the variables used in that computation changes).
We can make the variable that holds the filtered pokemon names into a reactive declaration. We’ll call this ‘suggestions’.
```javascript
    let suggestions: string[]
    $: suggestions =
        pokemonName.length > 0
            ? pokemonData.filter((name) => name.includes(pokemonName))
            : pokemonData
```
So, ‘suggestions’ is an array of pokemon names that includes the string entered in the input field.
The reactive assignment doesn’t work with typescript, so we can declare a ‘suggestions’ variable normally to preserve type checks.

### Loops
We’ll want to display the contents of this ‘suggestions’ array on the page, and we can do this by using a svelte loop. Svelte has a special keyword ‘each’ that enables us to display DOM elements for each item in the given iterable.
To display each pokemon name, simply use the each keyword to loop over the ‘pokemonData’ variable.
```javascript
<main>
<span>Search: </span>
 <input type="text" bind:value="{pokemonName}" />

 {#each suggestions as suggestion}
<h2>{suggestion}</h2>
{/each}
</main>
```
As we type into the input field, we can see the list of suggestions change. Pretty cool for such simple code.

### Conditional Rendering
Svelte has other keywords. Another useful one is #if.
The #if keyword allows for conditional logic.
For example, we can render a loading screen while we fetch the data from the pokeapi.
```javascript
<main>
 {#if pokemonData && pokemonData.length > 0}
<span>Search: </span>
 <input type="text" bind:value="{pokemonName}" />

 {#each suggestions as suggestion}
<h2>{suggestion}</h2>
{/each}
{:else}
<h2>Loading...</h2>
{/if}
</main>
```

### Components & Props
Props are used to pass data from one component to another. This is fairly standard for frameworks, and the syntax for this is very simple.
To signal that a component accepts a prop, an export keyword is used for a variable.
```javascript
<script lang="ts">
    export let stringProp: string
</script>
```
Now, to pass the value for ‘stringProp’, simply use the name of the exported variable when writing the component.
```javascript
<script lang="ts">
    import NewComponent from './NewComponent.svelte'
</script>

<NewComponent  stringProp="prop value"  />
```
For this app, let’s create a component for each suggestion.
Create a file ‘Suggestion.svelte’, and simply accept and display a ‘suggestion’ prop.
```javascript
<script lang="ts">
    export let suggestion: string
</script>

<div class="suggestion">{suggestion}</div>

<style>
    .suggestion {
        font-size: 1.25rem;
    }
</style>

```
Now, we can import this component and use it in our #each loop.
```javascript
import Suggestion from './Suggestion.svelte'
```
```javascript
{#each suggestions as suggestion}
<Suggestion suggestion="{suggestion}"/>
{/each}
```
This is pretty pointless at the moment, so let’s add some logic to the ‘Suggestion’ component in the form of events.

### Events
Custom events can be dispatched from one component to another. This allows us to have parent-child communication.
For our app, we want to be able to click on a suggestion to select our pokemon. We can do this by dispatching a custom event from the ‘Suggestion’ component to the App component, and then setting the value of a variable which holds our chosen pokemon.

First, create the new ‘chosenPokemon’ variable, and display it on the screen in App.svelte.
```javascript
let chosenPokemon: string = ''
```
```javascript
<h2>Chosen Pokemon: {chosenPokemon}</h2>
```

Now, in Suggestion.svelte, let’s dispatch a custom ‘chosePokemon’ event when clicking on a suggestion.
To create a custom event, we need to import the ‘createEventDispatcher’ from svelte.
```javascript
<script lang="ts">
    import { createEventDispatcher } from 'svelte'

    export let suggestion: string

    const dispatch = createEventDispatcher()
    const chosePokemon = (): void => {
        dispatch('chosePokemon', {
            pokemon: suggestion
        })
    }
</script>
```
We now have a ‘chosePokemon’ function that dispatches a custom ‘'chosePokemon' event to the parent component.
To call this function when clicking on a suggestion, we need to use a svelte event ‘on:click’ like this.
```javascript
<div class="suggestion" on:click="{chosePokemon}">{suggestion}</div>
```
Back in the App.svelte file, we can handle this custom event by using the on:(event name) syntax.
```javascript
<Suggestion suggestion="{suggestion}" 
on:chosePokemon="{(e) => {
chosenPokemon = e.detail.pokemon
}}"
/>
```
This handler sets the value of the chosenPokemon variable to be equal to the pokemon name passed in the custom event.
When we click on a suggestion, that pokemon name is shown.
I set the ‘chosenPokemon’ variable this way to introduce custom events, however, there is a much cleaner and easier way of doing this, bind forwarding.

Stores
Store api result items in store.

