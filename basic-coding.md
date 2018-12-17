### Can we code now?
Sure. 

We'll run through some basic code snippets to get you started with TypeScript. Don't forget TypeScript syntactically is JavaScript with type annotations. If you know JavaScript, this'll be pretty quick! If not, I'll try to get you started coding wise as quick as possible.

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
    // We have access to array methods here since!
    return numberStringArray.has(x => typeof(x) === "number") && numberStringArray.has(x => typeof(x) === "string");
}

```

```TypeScript
// Basic examples of expressions.
const numberValue = 5 + 5 * 5 / 2 - (-10); // 27.5
const stringValue = "Hello" + ",".concat(" ") + "world."; // "Hello, world."
const booleanValue = (5 < 3) || (numberValue == 27.5); // true
const functionWhichReturnsANumber: (number) => number = (x: number) => x;
let answer: number = functionWhichReturnsANumber(999);

// TypeScript infers this type as well!
const objectValue = {
    name: "TypeScript Cookbook",
    year: 2018,
    description: "best"
}; 
```

```TypeScript

// This is a type alias! It declares a type that we can reuse later called `Year`.
type Year = number | string[] | string;  
function giveYearBack(x: Year): Year {
    return x;
}

// This is an interface type. This is used to described 
// the structure of a specific object type.
interface DateInfo {
    month: number | string;
    year: number | string;
    day: number | string;
}

type Info = Year | DateInfo;

function printInfo(x: Info): void {
    console.log(x);
}

const currentDate: DateInfo = {
    month: 12,
    year: "2018",
    day: 13
};

if (currentDate.year === "2018") {
    console.log("This is the year this cookbook was originally written!");
}

printInfo(currentDate);
```


```TypeScript
class Shape {
    // something to note here is that the keyword public 
    // will automatically do `this.number = number;` for you. 
    public Shape(public numberOfSides: number, rounded: boolean) {
        this.rounded = rounded;
    }
} 
```