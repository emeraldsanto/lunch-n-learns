#### Built-in utility types

Convenient types included in the standard TypeScript library ❤️


<!-- Section 1 -->
##### Partial\<T\>

Returns a new type containing all properties of `T`, but nullable. `Required<T>` does the opposite.

```typescript [1-6|8-10|12-15]
interface User {
  _id: string;
  age: number;
  name: string;
  email: string;
}

function updateUser(fieldsToUpdate: Partial<User>): User {
  return { ...currentUser, ...fieldsToUpdate };
}

// Compiles successfuly since all keys are now optional.
updateUser({ age: 22 });
```


<!-- Section 2 -->
##### Readonly\<T\>

Returns a new type containing all properties of `T`, marked as read only (cannot be changed).

```typescript [1-8|10-11]
type ImmutableUser = Readonly<User>;

const user: ImmutableUser = {
  _id: "some-id",
  age: 22,
  name: "Yanick",
  email: "some-email"
}

// Does not compile, all properties are now read only.
user.name = "Yanick Bélanger";
```


<!-- Section 3 -->
##### Record\<TKey, TValue\>

Returns a new index type where all keys are of type `TKey` and have values of type `TValue`.

```typescript [1-5|7-8|10-11]
const countryNames: Record<string, string> = {
  ca: "Canada",
  fr: "France",
  us: "United States of America",
}

// We are able to access by index!
const usa = countryNames["us"];

// However, this is also allowed...
const uganda = countryNames["ug"];
```


<!-- Section 4 -->
##### Pick\<T, TKeys\>

Returns a new sub type of `T` with only the keys specified in `TKeys`.

```typescript [1-5|7-12]
interface User {
  _id: string;
  username: string;
  password: string;
}

type PublicUser = Pick<User, "_id" | "username">;

const user: PublicUser = {
  _id: "some-id",
  username: "emeraldsanto"
}
```


<!-- Section 5 -->

##### Omit\<T, TKeys\>

Returns a new sub type of `T` without all keys specified in `TKeys`.

```typescript [1-5|7-13]
interface User {
  _id: string;
  username: string;
  password: string;
}

// Much simpler than the previous example for complex types!
type PublicUser = Omit<User, "password">;

const user: PublicUser = {
  _id: "some-id",
  username: "emeraldsanto"
}
```


<!-- Section 6 -->
##### Parameters\<T\>

Returns a new type consisting of a tuple of all parameters of `T`.

```typescript
declare function callMe(maybe: boolean, when: Date): void;

// This evaluates to [boolean, Date]
type CallMeArguments = Parameters<typeof callMe>;
```


<!-- Section 7 -->
##### ReturnType\<T\>

Returns a new type matching the return value of `T`.

```typescript [1-3|5-6]
function create() {
  return { _id: "some-id", at: Date.now() }
};

// This evaluates to { _id: string, at: number }
type CreateReturn = ReturnType<typeof create>;
```