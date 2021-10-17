#### Custom type guards

A better alternative to type casting 💪


<!-- Section 1 -->
##### What is it?

A way to define special functions used to assert that a piece of data conforms to a certain type.


<!-- Section 2 -->
Let's start by defining a few types for our hypothetical application.

```typescript [1-5|7-9|10-13|16]
interface User {
  _id: string;
  role: string;
  username: string;
}

interface Customer extends User {
  role: "customer";
}

interface Admin extends User {
  role: "admin";
  doSomethingReallyDangerous(): void;
}

type ApplicationUser = Customer | Admin;
```


<!-- Section 3 -->
Then some custom reusable type guards.

```typescript [1-3|5-7]
function isCustomer(user: ApplicationUser): user is Customer {
  return user.role === "customer";
}

function isAdmin(user: ApplicationUser): user is Admin {
  return user.role === "admin";
}
```


<!-- Section 4 -->
Then later on...

```typescript [1-8|4-5]
declare const currentUser: ApplicationUser;

if (isAdmin(currentUser)) {
    // Inside this condition block, `currentUser` is of type `Admin`
    currentUser.doSomethingReallyDangerous();
}
```