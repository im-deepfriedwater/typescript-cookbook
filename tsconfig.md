### What is a TSCONFIG file
The TypeScript compiler is extensively configurable. It figures out what settings are on through parsing a file known as `tsconfig.json`. If the TypeScript compiler finds a `tsconfig.json` file then it automatically considers the directory it is in as the root directory of a TypeScript project. The following are various fields you can have in your file. Don't forget it is a JSON file, so we follow normal JSON syntax.

### Compiler Options
The options in here tell TypeScript how to compile your files. In here is where you can specify how strict or un-strict you prefer TypeScript to be when it checks your code.

Example of a `tsconfig.json` file with various compiler options:
```JSON
    {
        "compilerOptions": {
            "module": "commonJs",
            "noImplicitAny": true,
            "outDir": "./built",
            "target": "es2016",
            "strictNullChecks": true,
            "noImplicitReturns": true,
            "rootDir": "src"
        }
    }
```

We'll discuss each of the flags here. I will only be listing some of the possible values, as a full list of options and their options can be found on the official TypeScript handbook [here](https://www.typescriptlang.org/docs/handbook/compiler-options.html).

### module
- Type: string
- Example Possible values: `"CommonJS"`, `"AMD"`, `"ES2015"`...
- Description: Tells TypeScript what to transpile modules into.

### noImplicitAny
- Type: boolean
- Description: TypeScript will require all values for which TypeScript cannot do type inference (aka has an `any` type) to be declared explictly `any`.
- Example:
```TypeScript
function returnSelf(x) { // TypeScript will point this out as an error because `x` cannot be type inferred to a type.
    return x;
} 
```
We can fix this by doing:
```TypeScript02:07 AM
function returnSelf(x: any) {
    return x;
}
```

### outDir
- Type: string
- Example possible values: `"./output"`, `"./built"`
- Description: Tells TypeScript where to place its generated files after it compiles successfully.

### target
- Type: string
- Example possible values: `"ES5"`, `"ESNext"`, `"ES2015"`...
- Description: Tells TypeScript what to transpile the source code into. Allows you to use modern JS features that's backed by static checking for potentially dated platforms as well!

### strictNullChecks
- Type: boolean
- Description: Does not place `any` in the domain of other types besides the domain of the `null`, `undefined`, and `void` types. 
- Example:
```TypeScript
let favoriteNumber = 56; 
favoriteNumber = null; // TypeScript will error out here because `null` is not a part of the `number` type.
```
Can fix by doing:
```TypeScript
let favoriteNumber: number | null = 56; 
favoriteNumber = null;
```

This will force us to remember to deal with the possibility of `favoriteNumber` being `null` later on. It is a common mistake for developers to forget at some point a variable could be `null`. With this option, TypeScript will remember for you, and remind you about it later. 

### noImplicitReturns
- Type: boolean
- Description: Will error out when functions that are expected to return a value do not return a value in all code paths.
- Example:

```TypeScript
function printMoney(money: number) void {
    console.log(money);
} // No error in this function!
```

```TypeScript

function getMoney(): number { // Will error out here because we promised we would return a number value!

}
```

```TypeScript

function getMoney(id: string): number { // Will error out here because not ALL code paths return a number value.
    if (id === "TypeScript") {
        return 9999999;
    }

    return 0;
}
```

### rootDir
- Type: string
- Description: Tells TypeScript where the root directory of the files it needs to compile are. 

### strict
- Type: boolean
- Description: A useful convenience flag which turns on all strict type checking for you, such as `strictNullChecks` and so on. Check out the official handbook to see the full list of flags that get turned on.    



Again, a much more thorough and detailed table of all the possible compiler options for TSCONFIG can be found in the official TypeScript [handbook!](https://www.typescriptlang.org/docs/handbook/compiler-options.html)