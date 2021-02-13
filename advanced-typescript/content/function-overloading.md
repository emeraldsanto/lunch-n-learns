#### Function overloading

Polymorphism for functions 🤔


<!-- Section 1 -->
##### What is it?

It is a way for a function to have more than one signature while maintaining only one implementation. This allows for different behaviour and type-safe return types depending on the provided arguments.


<!-- Section 2 -->
##### What does it look like?

JavaScript (and therefore TypeScript) does not support proper function overloading as can be seen in other languages such as C# and Java. We can instead use multiple declarations, but only one implementation, to achieve the same effect.


<!-- Section 3 -->
##### The classic approach

The following implementation will simply return a union of all possible return types, or even `any` in more complex cases.

```typescript [1-7|9-11]
function reverse<T>(stringOrArray: string | Array<T>): string | Array<T> {
  if (typeof stringOrArray === "string") {
    return stringOrArray.split("").reverse().join("");
  }

  return stringOrArray.slice().reverse();
}

// Both are inferred as string | Array<string>
const shouldBeString = reverse("Yanick"); 
const shouldBeArray = reverse(["Y", "a", "n", "i", "c", "k"]);
```


<!-- Section 4 -->
##### The type-safe approach

The following implementation is vastly superior as TypeScript can infer the return type based on the arguments. We could also add separate JSDoc comments per definition.

```typescript [1-2|3-9|11-15]
function reverse(stringOrArray: string): string;
function reverse<T>(stringOrArray: Array<T>): Array<T>;
function reverse(stringOrArray: any): any {
  if (typeof stringOrArray === "string") {
    return stringOrArray.split("").reverse().join("");
  }

  return stringOrArray.slice().reverse();
}

// Inferred as string
const shouldBeString = reverse("Yanick");

// Inferred as Array<string>
const shouldBeArray = reverse(["Y", "a", "n", "i", "c", "k"]);
```


<!-- Section 5 -->
##### Real life example

This code was taken from the Moka API MongoDB wrapper utility, it consists of 10 overloads for a 3 line function 😅

```typescript [1-16|17-21]
async function execute<TModel = any>(collection: string, operation: 'aggregate', body: object[], parameters?: CollectionAggregationOptions): Promise<MongoOperationResult<'aggregate', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'countDocuments', body: FilterQuery<TModel>, parameters?: MongoCountPreferences): Promise<MongoOperationResult<'countDocuments', TModel>>;

async function execute<TModel = any>(collection: string, operation: 'findOne', body: FilterQuery<TModel>, parameters?: FindOneOptions<TModel>): Promise<MongoOperationResult<'findOne', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'find', body: FilterQuery<TModel>, parameters?: FindOneOptions<TModel>): Promise<MongoOperationResult<'find', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'findOneAndUpdate', body: FilterQuery<TModel>, parameters: TModel | UpdateQuery<TModel>, options?: FindOneAndUpdateOption<TModel>): Promise<MongoOperationResult<'findOneAndUpdate', TModel>>;

async function execute<TModel = any>(collection: string, operation: 'insertOne', body: Omit<TModel, '_id'>, parameters?: CollectionInsertOneOptions): Promise<MongoOperationResult<'insertOne', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'insertMany', body: Array<Omit<TModel, '_id'>>, parameters?: CollectionInsertManyOptions): Promise<MongoOperationResult<'insertMany', TModel>>;

async function execute<TModel = any>(collection: string, operation: 'updateOne', body: FilterQuery<TModel>, parameters: UpdateQuery<TModel> | Partial<TModel>, options?: UpdateOneOptions): Promise<MongoOperationResult<'updateOne', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'updateMany', body: FilterQuery<TModel>, parameters: UpdateQuery<TModel> | Partial<TModel>, options?: UpdateManyOptions): Promise<MongoOperationResult<'updateMany', TModel>>;

async function execute<TModel = any>(collection: string, operation: 'deleteOne', body: FilterQuery<TModel>, parameters?: CommonOptions & { bypassDocumentValidation?: boolean }): Promise<MongoOperationResult<'deleteOne', TModel>>;
async function execute<TModel = any>(collection: string, operation: 'deleteMany', body: FilterQuery<TModel>, parameters?: CommonOptions): Promise<MongoOperationResult<'deleteMany', TModel>>;

async function execute<TModel = any>(collection: string, operation: MongoOperations, body: unknown, parameters?: unknown, options?: unknown): Promise<unknown> {
  const db = await getInstance();

  return db.collection<TModel>(collection)[operation](body, parameters, options);
}
```