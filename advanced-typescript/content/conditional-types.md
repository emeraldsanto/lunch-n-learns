#### Conditional types

This type or that type? 🧐


<!-- Section 1 -->
##### What are they?

TypeScript provides us with a way to run conditions on types, much like the traditional ternary expressions, which are used on values.


<!-- Section 2 -->
##### What do they look like?

Here is a basic and not very useful example:

```typescript
type IsObject<T> = T extends object ? true : false;

declare const isArrayObject: IsObject<[]>; // true
declare const isStringObject: IsObject<string>; // false
```


<!-- Section 3 -->
##### Why should I use them?

Suppose you are writing a function that accepts either a normal type or a `Promise<Type>` and you want to return the final value.

Here's how you would implement this:

```typescript [1-3|5-6]
declare function maybeWaitFor<TValue>(
  value: TValue
): TValue extends PromiseLike<infer U> ? U : TValue;

const withoutPromise = maybeWaitFor("This is sync"); // string
const withPromise = maybeWaitFor(Promise.resolve("This is async")); // string
```


<!-- Section 4 -->
##### Real life example

This was once again taken from the Moka API MongoDB wrapper utility and allows us to map MongoDB operation name to its return type.

```typescript [1-12|14-36]
type MongoOperations =
  | 'aggregate'
  | 'countDocuments'
  | 'find'
  | 'findOne'
  | 'findOneAndUpdate'
  | 'insertOne'
  | 'insertMany'
  | 'deleteOne'
  | 'deleteMany'
  | 'updateOne'
  | 'updateMany';

export type MongoOperationResult<
  TOperation extends MongoOperations,
  TModel = any
> =
  TOperation extends 'aggregate'
  ? AggregationCursor<TModel>
  : TOperation extends 'countDocuments'
  ? number
  : TOperation extends 'findOne'
  ? TModel
  : TOperation extends 'find'
  ? Cursor<TModel>
  : TOperation extends 'findOneAndUpdate'
  ? FindAndModifyWriteOpResultObject<TModel>
  : TOperation extends 'insertOne'
  ? InsertOneWriteOpResult<TModel>
  : TOperation extends 'insertMany'
  ? InsertWriteOpResult<TModel>
  : TOperation extends 'updateOne' | 'updateMany'
  ? UpdateWriteOpResult
  : TOperation extends 'deleteOne' | 'deleteMany'
  ? DeleteWriteOpResultObject
  : any;
```