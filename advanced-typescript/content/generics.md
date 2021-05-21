#### Generics

For when you need more flexibility 🤸‍♀️


<!-- Section 1 -->
##### What are they?

Generics exist in every typed language and provide a way to code in a "type-agnostic" way.


<!-- Section 2 -->
##### What do they look like?

Any type can be made generic by using the `YourType<T>` syntax.

```typescript[2|3|1-5|6-7|9-11]
// Let's define the shape of our back-end responses
interface APIResponse<T> {
  data: T
}

// We can use it like so
const response: APIResponse<User> = await fetchOneUserFromAPI();

// Here, `response.data` is of type `User` 
console.log(response.username) // works!
console.log(response.email) // works!
```


<!-- Section 3 -->
##### Type constraints

They allow you to restrict which types can fit into your generics

```typescript[2|1-4|1-6]
// We only want to allow `APIResponse` and its subtypes in this function
function getData<T extends APIResponse<unknown>>(response: T) {
  return response.data;
}

getData<number>(3); // Error!
```


<!-- Section 4 -->
##### When should I use them?

They are especially useful for data structures

```typescript[1|2-6|9-10|12-13]
class Gift<T> {
  constructor(public content: T) {}

  open(): T {
    return this.content;
  }
}

const numberGift = new Gift<number>(666);
console.log(numberGift.open()); // 666

const stringGift = new Gift<string>('trampoline');
console.log(stringGift.open()); // trampoline
```