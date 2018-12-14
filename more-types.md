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
### Optional type

### Indexable type
### Class type