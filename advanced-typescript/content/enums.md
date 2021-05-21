#### Enums

Representing sets of known values ℹ️


<!-- Section 1 -->
##### What are they?

They help us define and give meaning to predefined arbitrary values while also making them easier to access in a type-safe way, effectively solving the "magic string/number" problem.


<!-- Section 2 -->
##### Enum members

An enum can be made up of two types of members (values), constant and computed.

```typescript[1-4|6-10|11-14]
enum Constant {
  A = 1,
  B = 'magic-string'
}

enum Computed {
  A = "myString".length,
  B = myFunction()
}

declare function giveMeAnEnum(x: Constant | Computed): void;

giveMeAnEnum(Constant.A);
giveMeAnEnum(Computed.B);
```


<!-- Section 4 -->
##### `const` enums

There exists a special kind of enum, commonly referred to as `const` enum, which allows us to reduce the overhead that comes from compiling enums to plain JavaScript.

*N.B. This is only applicable to enums with exclusively constant members.*


<!-- Section 5 -->
Let's look at the generated JavaScript code for our previous example:

```js
"use strict";
var Constant;
(function (Constant) {
    Constant[Constant["A"] = 1] = "A";
    Constant["B"] = "magic-string";
})(Constant || (Constant = {}));
giveMeAnEnum(Constant.A);
```


<!-- Section 6 -->
Now, let's add the `const` modifier to our declaration:

```typescript
const enum Constant {
  A = 1,
  B = 'magic-string'
}
```


<!-- Section 7 -->
And we are left with the following:

```js
"use strict";
giveMeAnEnum(1 /* A */);
```

**The `const enum` is completely stripped from the code** and its **members are inlined at each usage site**. This makes your code lighter, and more readable when dealing with production code (compiled, minified) on the web for example. 


<!-- Section 8 -->
##### Alternatives


<!-- Section 9 -->

**Union types**

In some cases, you may prefer to use a union type to achieve the same result:

```typescript
type Role = 'user' | 'admin' | 'superadmin'

declare function union(x: Role): void;

union('admin');
```


<!-- Section 10 -->
**Object + `as const`**

Or simple objects to match the JavaScript spec more closely:

```typescript
const Role = {
  User: 'user',
  Admin: 'admin',
  SuperAdmin: 'superadmin',
} as const;

type Role = typeof Role[keyof typeof Role];

declare function union(x: Role): void;

union(Role.Admin);
```