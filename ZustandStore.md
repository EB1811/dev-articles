# Stop Overcomplicating your State – Try Zustand
With the advent of hooks, a lot of React developers have started to move away from Redux as the default state management of choice.\
Many new state managers have become popular. One of which is [Zustand](https://github.com/pmndrs/zustand).\
Zustand is a modern state manager that fits nicely in this world of hooks. It is lightweight (only 66.4 kB unpacked), fast, and hooks-based. The brilliance of Zustand is that it’s simple yet powerful.

I stumbled upon Zustand when re-designing our app’s state management. The complexity of Redux and immaturity of React Context made us want to move to another state manager. Having been burned by Redux’s aforementioned complexity, Zustand drew me in with its promised simplicity, describing itself as a ‘barebones’ state manager.

I want to highlight some key aspects of Zustand, and show how it can be used in a project.

I'm going to be demonstrating Zustand using my testing project [starwars-searcher](https://github.com/EB1811/starwars-searcher). This is a very simple app that utilizes the [swapi api](https://swapi.dev/) for a live search feature. Lets extend this project to retrieve star wars planet names from the api and display them on a list, using Zustand as our state manager.

## How to create a store
First, let's install Zustand.
```
yarn add zustand # or npm install zustand
```
Creating a store is a very simple process.
We'll use Zustand's 'create' to make a react hook which we will call 'useStore'. I’ll avoid typing for now (we’ll talk in depth about using zustand with typescript soon).
```
import create from "zustand";
export const useStore = create<any>(
    set => ({
    })
);
```
Now we can to set the store's initial state.\
We'll create a variable to store planet names, and a function to set that variable.
```
export const useStore = create<StoreType>((set) => ({
    planetNames: [],
    setPlanetNames: (data: any) => set({ planetNames: data })
}));
```
And that's it!\
With our store created, let's import it into a React component to store planet names from the swapi api.\
```
const planetNames = useStore((state) => state.planetNames);
const setPlanetNames = useStore((state) => state.setPlanetNames);
 
useEffect(() => {
    const populatePlanetsFromAPI = async () => {
        const planetsData = await (
            await fetch("https://swapi.dev/api/planets")
        ).json();
        setPlanetNames(planetsData.results.map((pd: any) => pd.name));
    };

    populatePlanetsFromAPI();
}, []);
```
```
return (
    <div>
        <h1>Planet Names</h1>
        <ul data-testId='planets-list'>
            {planetNames.map((name: any) => (
                <li key={name} data-testId={`planet-${name}`}>
                    {name}
                </li>
            ))}
        </ul>
    </div>
);
```
As you can see, it's very easy to set up a Zustand store.

### Async Actions
Of course, a real world application utilizes asynchronous actions, something which is rather frustrating in redux.\
In Zustand however, performing asynchronous actions has no additional complexity. Simply tag make the store's funciton as async, and use the await keyword to wait for actions to finish.
We'll move the fetch from the useEffect to the store by adding a retrieve function.
```
retrievePlanetNames: async () => {
    const planetsData = await (
        await fetch("https://swapi.dev/api/planets")
    ).json();

    set({ planetNames: planetsData.results.map((pd: any) => pd.name });
}
```
We can now simply use this function in the useEffect.

## Equality
You can define how Zustand checks equality between objects by passing in an equality function as the second parameter. By default, properties are compared with strict-equality, but we can compare using shallow checks by passing in Zustand’s shallow function. The differences between default and shallow are demonstrated below.
You can also create your own comparison function for greater control over re-rendering.
```
// Same behaviour when values are primitives.
Object.is(1, 1) // True
shallow(1, 1) // True

// But when values are objects:
Object.is({number: 1}, {number: 1}) // False
shallow({number: 1}, {number: 1}) // True
```

## Middleware
Another awesome feature of Zustand is the ability to create middleware to add additional features to your store. For example, you can easily create middleware to log state changes.
```
const log = config => (set, get, api) => config(args => {
  console.log("Applying", args)
  set(args)
  console.log("New State", get())
}, get, api)
```

### Redux Dev Tools
With the middleware functionality, we can easily actually use an amazing extension created for Redux, Redux DevTools [link](https://github.com/zalmoxisus/redux-devtools-extension). We just need to import the devtools middleware, and attach it to our store.
```
import { devtools } from "zustand/middleware";

export const useStore = create<any>(
    devtools((set) => ({
        planetNames: [],
        getPlanetNames: async () => {
            const planetsData = await (
                await fetch("https://swapi.dev/api/planets")
            ).json();

            set({ planetNames: planetsData.results.map((pd: any) => pd.name) });
        },
        setPlanetNames: (data: any) => set({ planetNames: data })
    }))
);
```
Now we can visually see everything stored, and look through the store’s timeline, which is very cool and useful.

### Immer + Typescript.
[Immer](https://github.com/immerjs/immer) is another great package that makes reducing nested structures easy.\
We can create middleware to allow us to use immer easily. Here is a fully typed version.
```
import create, { State, StateCreator } from "zustand";
import produce, { Draft } from "immer";

export type StoreType = {
    readonly planetNames: string[];
    getPlanetNames: () => Promise<void>;
    setPlanetNames: (data: any) => void;
};

const immer =
    <T extends State>(config: StateCreator<T>): StateCreator<T> =>
    (set, get, api) =>
        config(
            (partial, replace) => {
                const nextState =
                    typeof partial === "function"
                        ? produce(partial as (state: Draft<T>) => T)
                        : (partial as T);
                return set(nextState, replace);
            },
            get,
            api
        );

export const useStore = create<StoreType>(
    devtools(
        immer((set, get) => ({
              planetNames: [],
              getPlanetNames: async () => {
                const planetsData = await (
                    await fetch("https://swapi.dev/api/planets")
                ).json();

                set({ planetNames: planetsData.results.map((pd: any) => pd.name) });
              },
              setPlanetNames: (data: any) => set({ planetNames: data })
        }))
    )
);
```
## Store Slices.
When working with Zustand, your store might become quite dense. Keeping all of your app’s state in one file becomes unfeasible.\
Luckily, you can easily split your store into various functions to keep your files small and manageable.
Here’s a simple example from Zustand’s docs.
```
import create from 'zustand'

const createBearSlice = (set, get) => ({
   eatFish: () => set((prev) => ({ fishes: prev.fishes > 1 ? prev.fishes - 1 : 0}))
})

const createFishSlice = (set, get) => ({
   fishes: 10
})

const useStore = create( (set, get) => ({
    ...createBearSlice(set, get),
    ...createFishSlice(set, get)
}))
```
As you can see, store slices can interact with each other. However, if we want to keep slices separate, we can set up typescript to not allow slices to interact with each other.

In my testing project, I have a few more variables and functions in my store. These are used to get people, planet, and species data from the swapi api for a live search page [(link)](https://github.com/EB1811/starwars-searcher/blob/master/starwars-searcher/src/components/search/SearchPage.tsx).\
As an exercise, we’ll separate data used for this functionality from the planet names list we created in this article.\
Here is the store slice for our planet names data with typescript.
```
import { GetState, SetState, StateCreator, StoreApi } from "zustand";

export interface PlanetNamesSlice {
    readonly planetNames: string[];
    getPlanetNames: () => Promise<void>;
    setPlanetNames: (data: string[]) => void;
}

const createPlanetNamesSlice:
    | StateCreator<PlanetNamesSlice>
    | StoreApi<PlanetNamesSlice> = (set, get) => ({
    planetNames: [],
    getPlanetNames: async () => {
        const planetsData = await (
            await fetch("https://swapi.dev/api/planets")
        ).json();

        set({ planetNames: planetsData.results.map((pd: any) => pd.name) });
    },
    setPlanetNames: (data: string[]) => {
        set({ planetNames: data });
    },
});

export default createPlanetNamesSlice as (
    set: SetState<PlanetNamesSlice>,
    get: GetState<PlanetNamesSlice>,
    api: StoreApi<PlanetNamesSlice>
) => PlanetNamesSlice;
```
And we can use it to create our central store like so.
```
interface IStore extends PlanetNamesSlice, StarWarsDictSlice {}

export const useStore = create<IStore>(
    devtools(
        immer((set, get, api) => ({
            ...createPlanetNamesSlice(
                set as unknown as SetState<PlanetNamesSlice>,
                get as GetState<PlanetNamesSlice>,
                api as unknown as StoreApi<PlanetNamesSlice>
            ),
            ...createStarWarsDictSlice(
                set as unknown as SetState<StarWarsDictSlice>,
                get as GetState<StarWarsDictSlice>,
                api as unknown as StoreApi<StarWarsDictSlice>
            ),
        }))
    )
);

```
Now you have a much cleaner store with types and typescript enforcement of slice separation.

## Testing your store:
To test our store using jest, we’ll need some packages.\
[React testing library](https://testing-library.com/)\
[Rect testing - hooks](https://github.com/testing-library/react-hooks-testing-library)

With react-hooks-testing, it’s very easy to test the functions of our store.\
One important thing to know is that the store’s state is kept between tests. We can deal with this in many ways. One way is to set the content of the store before each test, and another is to set up a mock of Zustand which resets the store each time; you can decide which route to take.\
Now let's test our set function:
```
import { act, renderHook } from "@testing-library/react-hooks";
import { cleanup } from "@testing-library/react";
import { useStore } from "./useStore";

describe("useStore", () => {
    afterEach(() => {
        // You can chose to set the store's state to a default value here.
        jest.resetAllMocks();
        cleanup();
    });

    it("The setPlanetNames function correctly sets the planetNames variable.", () => {
        const { result } = renderHook(() => useStore((state) => state));

        act(() => {
            result.current.setPlanetsData(["earth"]);
        });

        expect(result.current.planetsData).toEqual(["earth"]);
    });
});
```
As you can see, it’s very easy to unit test our store.

In case you are wondering how to test components that use the store, we can easily mock our store with the required returned values.
```
it("Component gets data from the store.", async () => {
    jest.spyOn(Store, "useStore").mockImplementation((fn) =>
        fn({
            planetNames: ["Tatooine", "Mandalore"],
            infoDict: {},
            infoNamesArr: [],
            setPlanetNames: (data) => {},
            getPlanetNames: async () => {},
            populateWithAPI: async () => {},
        })
    );

    render(<PlanetsMap />);

    const listOfPlanets = screen.getByTestId("planets-list");
    expect(listOfPlanets.children).toHaveLength(2);

    expect(screen.queryByTestId("planet-Tatooine")).toBeTruthy();
    expect(screen.queryByTestId("planet-Mandalore")).toBeTruthy();
});
```
I believe that the ease of testing is a great benefit of Zustand.

## Final Notes
In my opinion Zustand is a very refreshing state manager. The absence of boilerplate makes it such a nice option for personal projects where one doesn’t want to spend an afternoon setting up a store with a single variable.\
However, this is not to say that Zustand is only suitable for small, personal projects. Having worked with Zustand in a real production environment, its advanced features make it a powerful tool on par with something like Redux.\
While seemingly basic, custom equality functions, middleware, and store slices can make Zustand a strong tool for central state management.\
Looking at some of the downsides, Zustand's middleware is very type unfriendly, an issue that Zustand is currently looking at [(link)](https://github.com/pmndrs/zustand/pull/601). Also, Zustand's documentation could use some more work, such as showing how to handle nested state, how best to test our stores, and detailing how to handle multiple middlewares.

### Other Options
Nowadays, there's quite a bit of options for central state management; Jotai, Recoil, and React-query, among others. I haven’t looked into these, but would like to in the future.

What do you think? Does zustand sound like something you’d like to use, or do you really like your current state manager?

---
If you enjoyed this article, please consider sharing it.\
Checkout my [github](https://github.com/EB1811) and other [articles](https://dev.to/emmanuilsb)
