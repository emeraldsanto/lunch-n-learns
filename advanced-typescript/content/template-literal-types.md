#### Template literal types

Autocorrecting strings with types? 📚


<!-- Section 1 -->
##### What are they?

Added in TypeScript 4.1 (November 2020), they are stricter composable string types.


<!-- Section 2 -->
##### What do they look like?

They are just good ol' template strings, except we can now put types in them...

```typescript
type Name = "Yanick";
type JobTitle = "Full Stack Developer";

// "Yanick, Full Stack Developer"
type NameAndJobTitle = `${Name}, ${JobTitle}`;
```


<!-- Section 3 -->
##### Why should I use them?

Let's say make sure all user tags match a specific format (username followed by # and four numbers).

```typescript [1|3-4|6-7]
type UserTag = `${string}#${number}`;

// All good here!
const valid: UserTag = "EmeraldSanto#1234";

// Compilation error!
const invalid: UserTag = "EmeraldSanto";
```


<!-- Section 4 -->
##### Real life example

You're building a UI library or framework which exposes CSS classes for all colors and their shade variants. That's gotta be hard to type by hand? Say no more:

```typescript [1-11|13-22|24-27|29-33]
type ColorName =
  | "black"
  | "blue"
  | "brown"
  | "gray"
  | "green"
  | "orange"
  | "red"
  | "pink"
  | "purple"
  | "yellow";

type ColorShade =
  | 100
  | 200
  | 300
  | 400
  | 500
  | 600
  | 700
  | 800
  | 900;

// This results in a type matching all
// `ColorName` with all `ColorShade`.
// This represents 90 combinations 🤯
type Color = `${ColorName}-${ColorShade}`;

// All good here!
const niceColor: Color = 'blue-400';

// Compilation error!
const invalidColor: Color = 'green-1200';
```