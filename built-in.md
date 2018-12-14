### Built-in types
Types in programming languages can accurately be described as sets of values. We'll be taking a look at the fundamental types built-in to TypeScript to tell us what we can use in our type annotations.
_________________

### Boolean
A basic type, it represents the set of values `true` and `false`. 
```TypeScript
let logicalResult: boolean = true;
logicalResult = false;
logicalResult = 5 > 3;
```

### Number
TypeScript does not differentiate between different floats, doubles, integers etc. More accurately, all numbers are floating point values.
```TypeScript
let numberResult = 5 + 3 - 1;
numberResult = 0.444444444 * -1;
```

### String
The string type represents text. There is no smaller type such as the character type. Thus, it can be zero or more characters surrounded by single quotes `'test'` or double quotes `"test"`.
```TypeScript
let aString = "goodbye.";
aString = 'so soon';
```

### Any
The `any` type can be used to opt out of type checking. "Any" matches with all types. It can also be used to describe values that we might not know the type of at the time of writing. Or potentially "catch" all functions. As the official TypeScript [documentation](https://www.typescriptlang.org/docs/handbook/basic-types.html) notes on the `any` type, it can be used while porting existing to JavaScript code to TypeScript, and then later one may go back and fill in the types gradually. 

```TypeScript
let anything: any = "this";
anything = 'can';
anything = 4;
anything = false;
anything = ["be", "anything"];
```

### Array
The `array` type represents a collection of values of homogeneous types. The following are equivalent ways to declare an `array` type.

```TypeScript
const numberArray: number[] = [1, 2, 3];
```

```TypeScript
const numberArray: Array<number> = [1, 2, 3];
```
```TypeScript
const multiTypeArray: (number | string)[] = [1, 2, "3"];
const anyTypeArray: any[] = [1, 2, "uh", "typescript", true];
```

### Tuple
The `tuple` type is similar to array, but have a `fixed` length. They can also be heterogeneous. Useful for when you will know confidently what the length will be of the collection every time. 

We'll see elements of place-oriented programming here as order does matter.

```TypeScript
let tupleValue: [number, string, string];
tupleValue = ["brook", "crook", -43]; // TypeScript will error out here!
tupleValue = [-41, "cook", "book"];
```

### Enum
Enums give you a collection of labelled resuable values. 

```TypeScript
enum Directions {Up, Down, Left, Right}
let currentDirection = Directions.Up;

if (currentDirection === Directions.Up) {
    console.log("It's up!");
} else if (currentDirection === Directions.Left) {
    console.log("Same as my non-dominant hand.");
} else if (currentDirection === Directions.Right) {
    console.log("No comment.");
} else {
    console.log("We don't believe in this direction.");
}
```

### Void
`void` is normally used to describe the return values of functions that do not return anything. It *can* be used to describe variable types. This describes the set of only `null` or `undefined`.

```TypeScript
function doSomething(): void{
    console.log("I did it.")
};

doSomething();

const x: void = undefined;
```

### Null and Undefined
`null` and `undefined` are both the only members of their respective sets. However, they are subtypes of all other built-in types *unless* users remember to turn on `strictNullCheck` (as recommended by the [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/basic-types.html)). With this flag on `null` and `undefined` are no longer subtypes of other types.

This is recommended because it address the `million dollar mistake`! Normally, `null` and `undefined` as part of other types allows these values to propagate through as developers commonly forget to account for these values coming up in their program. These commonly result in `null-reference exceptions`. But, turning the flag on will force developers to account for the possibility of `null` and `undefined`. We cannot really use `null` and `undefined` on their own, but with the `strictNullCheck` on we can union these types with other built-ins to force us to remember to handle nullable types.

```TypeScript
// Assuming we have `strictNullCheck` on:
function compute(n: number | null): number{
    return n + 5; // Won't compile! TypeScript knows this could be possibly `null`!
}
```

```TypeScript
// Assuming we have `strictNullCheck` on:
function computeButZeroIfNull(n: number | null): number{
    if (n === null) { 
        return 0;
    }
    // An example of control-flow based analysis! TypeScript can reason here
    // that we checked if it was null earlier, so here `n` MUST be number at 
    // this point.
    return n + 5; 
}
```

### Never
A `never` value can never be assigned to ANY other type, even `any`. It is meant to describe values which in code that will be reached.

```TypeScript
function throwAnError(): never {
    throw new Error("This function will never return a value.");
}
```

### Object
`object` is a type meant for making built-in library functions make better sense in this type system. Additionally, they represent the set of values of non-primitives such as NOT `number`, `boolean`, `string` etc.
