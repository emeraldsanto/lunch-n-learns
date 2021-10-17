#### Let's start with the basics


<!-- Section 1 -->
##### Defining variables

```typescript [1-2|4-5]
// Mutable
let age = 22;

// Immutable
const name = "Yanick";
```


<!-- Section 2 -->
##### Defining functions

```typescript [1-3|5-7]
function aBoundFunction(...args: Array<string>): void {
  // Do some work
}

const anArrowFunction = (...args: Array<string>): void => {
 // Do some work
}
```


<!-- Section 3 -->
##### Defining custom types

```typescript [1-5|8|13-17]
type Make =
  | "Honda"
  | "Hyundai"
  | "Mitsubishi"
  | "Subaru";

interface Car {
  make: Make;
  model: string;
  price: number;
}

const myDreamCar: Car = {
  make: "BMW", // Error!
  model: "X5",
  price: 30_000
}
```