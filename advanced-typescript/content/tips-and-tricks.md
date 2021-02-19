#### Tips and tricks

Some of my favourite TypeScript code snippets ✂️


<!-- Section 1 -->
##### Type safe error handling

By default, the `error` parameter of a catch block is typed as `any` but since TypeScript 4.0 (August 2020), we are allowed to explicitly type it as `unknown` which enforces type checking before accessing any properties.

```typescript [2|3|4-8]
try {
  await somePotentiallyThrowingFunction();
} catch (error: unknown) {
  if (error instanceof Error) {
    console.error(`An error occured - ${error.message}`);
  } else {
    console.error(`An error occured - ${JSON.stringify(error)}`);
  }
}
```


<!-- Section 2 -->
##### Typing a non-empty array

By default, all `T[]` and `Array<T>` types will allow empty arrays, but what if you want an array to have at least one element?

```typescript [1|3-4|6-7]
type NonEmptyArray<T> = [T, ...Array<T>];

// Compilation error!
const empty: NonEmptyArray<string> = [];

// All good here!
const full: NonEmptyArray<string> = ["That's nicer!"];
```


<!-- Section 3 -->
##### Typing class constructor parameter

In some cases, you might want to receive a class constructor as a function parameter, not an instance of the class itself.

```typescript [1-5|7|9-10]
function initialize<
  T extends { new (...args: any[]): InstanceType<T> }
>(constructor: T): InstanceType<T> {
  return new constructor();
}

const today = initialize(Date);

// All good here, today is an instance of `Date`!
today.getTime();
```


<!-- Section 4 -->
##### Typing multiple properties of the same type

TypeScript allows us to use a special syntax to define multiple properties sharing a common type.

- This only works with `type` and not `interface`.
- It is not possible to define other properties of different types using this syntax.

```typescript [1-3|5-16]
interface Continent {
  countries: Array<string>;
}

type Continents = {
  [K in
    | "africa"
    | "antarctica"
    | "asia"
    | "europe"
    | "northAmerica"
    | "oceania"
    | "southAmerica"
  ]: Continent;
}
```