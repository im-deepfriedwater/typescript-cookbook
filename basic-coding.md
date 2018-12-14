### Can we code now?
Sure. 

We'll run through some basic code examples to get you started with TypeScript. Don't forget TypeScript syntactically is JavaScript with type annotations. If you know JavaScript, this'll be pretty quick! If not, I'll try to get you started coding wise as quick as possible.

To test code quickly without setting up a project use the [TypeScript Playground!](https://www.typescriptlang.org/play/index.html)

```TypeScript
const a = ""; 
// TypeScript can automatically type infer this to be a string.
// Additionally, since the variable `a` is `const` it can never be reassigned.
const b: string = "If you really feel the need to, you can type this. There's *not* really a need.";
// A function that takes in parameter `message` typed as a string and does not return anything.
function print(message: string): void {
    console.log(message);
}

print("Hello, world!");
print("             ");
print(a);
print(b);
print(11222); // This will be flagged as an error by TypeScript!
```

```TypeScript
// Variables declared with let can be reassigned and mutated freely!
let numberStringArray: (number | string)[]; 
numberStringArray.push(5);
numberStringArray.push("5");

function checkForANumberAndString(numberStringArray: (number | string)[]): boolean {
    return numberStringArray.has(x => typeof(x) === "number") && numberStringArray.has(x => typeof(x) === "string");
}
```

```TypeScript
// Basic examples of expressions.
const numberValue = 5 + 5 * 5 / 2 - (-10); // 27.5
const stringValue = "Hello" + ",".concat(" ") + "world."; // "Hello, world."
const booleanValue = (5 < 3) || (numberValue == 27.5); // true

// TypeScript infers this type as well! We'll see how to codify this in the                     
// `type interface` section! 
const objectValue = {
    name: "TypeScript Cookbook",
    year: 2018,
    description: "best"
}; 

```

