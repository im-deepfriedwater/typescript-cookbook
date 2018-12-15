### How to do more interesting things with types
We've seen some trivial examples of TypeScript's type system. It is time to expand on that! We will be exploring more interesting features in the type system and how these can be used practically in programs.

### Type alias
If we want to reuse type declarations, we can declare them using `type nameOfType = number | string`. Now, the type `nameOfType` has been bound to the union type of `number` and `string`.

### Interface type
To describe the type of objects in TypeScript, we use interface types. TypeScript follows the principle of structural types, sometimes known as duck typing. Types are not compatible just because we *say* they are. Types are compatible *if* they are. For example:

```TypeScript
interface Vehicle {
    grounded?: boolean;
    numberOfWheels: number;
    model: string;
}

// TypeScript will check to make the object literal we are assinging
// has the *exact* same properties and nothing else!
const car: Vehicle = {
    numberOfWheels: 4,
    grounded: true,
    model: "best model"
};

// TypeScript will point out an error here because the variable `bike`
// does *not* have model. 
const bike: Vehicle = {
    numberOfWheels: 2,
    grounded: true
};

// TypeScript will point out an error here because the variable `boat`
// has an extra property and thus is not the same structural as the
// vehicle type. 
const boat: Vehicle = {
    numberOfWheels: 0,
    model: "best boat",
    grounded: true,
    harbor: "west"
};


Interface types can use the keyword `extends` on  other interfaces. This copies the properties to allow you to reuse types.

```TypeScript

interface Vehicle {
    wheels: number;
    model: string;
}

interface Airplane extends Vehicle {
    airline: string;
}

const plane: Airplane = {
    wheels: 5,
    model: "fast",
    airline: "cheap"
}
```

### Union type
Union types describe the set for values that are compatible to all types that are in the union. The `|` is used to describe the members of the union. For example, `number | boolean | string` describes the union type that is compatible with values that are compatible with either `number`, `boolean`, or `string`.

If there are functions that we expect to take a `string` or `number` we might use the union type to describe this. Union types also work with interface types. They only allow you to use members that are in all types of the union.

```TypeScript
interface Square {
    color: string,
    width: number
}

interface Circle {
    radius: number,
    color: string
}

const a: Square | Circle = {
    radius: 4,
    color: "red"
};

let colorResult = a.color; // At this point, `a` is compatible with the `Circle` type specifically so
let radius = a.radius;     // we are able to access properties of the `Circle` interface.

function generateAShape (): Square | Circle {
    return { color: "green", width: 5 };
}

const g = generateAShape();
g.color; // We can access the color property because it is common to both Square AND Circle.
g.width; // We cannot access the width property because we cannot guarentee this property will exist.
```

### Intersection type
Intersection types describe a set that is compatible with objects that are a combination of all the types that are being intersected. This can be used when you need an object that has the properties of ALL types in the intersection. 
```TypeScript
interface Square {
    color: string,
    width: number
}

interface Circle {
    radius: number,
    color: string
}

type intersectionType = Square & Circle;

function generateAShape (): intersectionType {
    return { color: "blue", width: 10, radius: 5 };
}

const result = generateAShape();
console.log(result.color, result.width, result.radius);
```


### Generics
Sometimes we do not know what the exact type will be, but we do know what we need to do with it! In that case we can use generics. When we're declaring a generic type, we use type parameters that we take in.

```TypeScript
// Not too useful, we're simply 
// giving the type back to them.
type genericExample<T> = T; 
const genericValue: genericExample<number> = 5; 

interface GenericInterface<T> {
    genericValue: T
}

const genericInterfaceExample: GenericInterface<string> = {
    genericValue: "example of a generic thing"
};
```


Inspired by an example from the [TypeScript handbook.](https://www.typescriptlang.org/docs/handbook/advanced-types.html) This is an example of types being recursive as you can see it refers to itself. 

```TypeScript
// Perhaps a more useful example.
// We can use generics to represent
// a tree data structure.
interface LinkedTree<T> {
    leftSubTree: LinkedTree<T>,
    rightSubTree: LinkedTree<T>
    innerValue: T,
}
```

Fun fact, you might think that this type is infinitely recursive. As to figure out what a `leftSubTree: LinkedTree<T>` is, the compiler would refer back to the definition and see again that it is an interface that contains a `leftSubTree` of type `LinkedTree<T>` and so on and so on.

Turns out, the designers behind TypeScript solved the problem by following their principle that they don't need 100% soundness. They decide to cut off checking at 5 levels of recursion and just trust the type at that point. (They showcased this in their TypeScript 2018 Build video which can be found [here](https://youtu.be/hDACN-BGvI8)).

### Conditional type
One of the more esoteric but powerful type features in TypeScript. Conditional types specify the type based on a condition. The quickest way to see this is with examples:

```TypeScript
// This line essentially says grab the type of the elements of
// the array. `T extends Array` means the type parameter must 
// be compatible with the `array` type. `<infer U>` means 
// the type parameter `U` will be inferred from the member type
// of the array. 
type Flatten<T> = T extends Array <infer U> ? U : T;
const a: Flatten<number[]> = 5; // number type
```

```TypeScript
// We can also exclude types, and say "we accept certain things BUT NOT this type".
type Exclude<T, U> = T extends U ? never : T;
type T1 = Exclude<number | string | (() => void), Function>; // number | string
```

### Optional type
In the cases where you want a parameter to be optionally passed in, we can denote these using a `?`.

```TypeScript
interface Square {
    width: number;
    color: string;
}

function generateSquare(width: number, color?: string) {
    return {
        width: width,
        color: color ? "white" : color
    };
}         

// By default if a parameter doesn't get passed in but
// it is flagged as an optional parameter it is given
// a value of undefined.
const callResult = generateSquare(100);
const greenSquare = generateSquare(20, "green");
```

### Index type
There are cases where you might want to type something based on the properties of an interface type. These might look a bit harder to understand but I'll do my best to comment what's happening. The `keyof` operator can be used to refer to something as the union of the properties of the type. See below! 

```TypeScript
interface Vehicle {
    wheels: number;
    grounded: boolean;
    name: string;
}

let b: keyof Vehicle;
b = "wheels";
b = "grounded";
b = "name";
b = "model"; // Errors out here! It *must* be a property of the `Vehicle` interface.
```

The `keyof` keyword creates a union between the names of the properties of the `Vehicle` interface type. If you add properties later on to the `Vehicle` type, the type of `b` will automatically be updated to have the new properties at compile time.

You can also type the members of a mapping. `[key: string]: number`, describes a mapping that takes in keys only of the `string` type that map to values only of the `number` type. 

```TypeScript
// A reminder you can name the type parameter anything you want, common convention is to name T for type. But for 
// demonstration purposes we'll name it M here.
interface NumberedMapping<M> {
    [key: string]: number;
}

interface NumberedMapping<M> {
    [key: number]: M;
}

const a: NumberedMapping<boolean> = {
    1: false,
    2: true,
    3: true
};

const b: NumberedMapping<boolean> = {
    10: true,
    13: false,
    99: "false" // TypeScript will point out this isn't a boolean!
};
```

### Classes
