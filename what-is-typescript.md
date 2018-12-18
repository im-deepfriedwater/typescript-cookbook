### What?
The official tagline for TypeScript is "JavaScript that scales!" It justifies this tagline through these two features:
    1. TypeScript is a superset of JavaScript.
    2. TypeScript is a statically-typed programming with powerful control-flow based type analysis.

### How is it a superset?
All valid JavaScript programs are valid TypeScript programs! TypeScript adds optional typing that users can buy into as much as they want to.

```TypeScript
// Super simple TypeScript example.
function addNumbers(x: number, y: number): number {
    return x + y;
}

const z: number = addNumbers(1, 2);
```

```TypeScript
// Same example as above, but no types.
function addNumbers(x, y) {
    return x + y;
}

const z = addNumbers(1, 2);
```

The two examples above are *both* valid TypeScript programs. The type annotations are *optional*. Perhaps unsuprisingly, writing TypeScript without types is basically just writing JavaScript. 

### What the heck does control-flow based type analysis mean?

When doing type analysis, TypeScript reads through your code line by line parsing your code to figure out the type of a variable to give the most precise type possible.

For example:
```TypeScript

function handleNumbersOrStrings (numberOrString: number | string): void {
    let numberSum: number = 5;
    let resultString: string = "what is typescript";
    if (typeof numberOrString === "number") {
        numberSum += numberOrString;
    } else { 
        resultString += numberOrString.toLowerCase();
    }
}
```

In the above example TypeScript can do some really cool stuff with this control-flow based type-analysis. While checking your code, it'll read it line by line as I've said before. But something interesting happens when it gets into these if else clauses. Within the if statement, TypeScript can reason that the type of `numberOrString` MUST be a `number` since it can only get inside here IF the type of `numberOfString` is `number`. Thus, TypeScript can guarantee that `numberSum` will remain a number as we are just adding another number to it. Similarly for the else clause, TypeScript will reason that `numberOfString` was typed to be one of two types. Inside this else clause it will realize that `numberOrString` MUST be a `string` here, therefore it has access to string methods such as `toLowerCase();`. (You can check me yourself by throwing this function into the official TypeScript Playground here! [Link](https://www.typescriptlang.org/play/index.html))

Without this control-flow based type-analysis the type analysis must just reason `numberOrString` is just the type `number | string` because it cannot get any more accurate.

These type checks manifest themselves either on the IDE (VSCode is built to go really well with TypeScript) you are using, or in console output when you attempt to compile TypeScript.

### But why does this make JavaScript SCALE?

The ability to buy-in is quite powerful. The more type annotations we add to our code the more TypeScript can typecheck for us. TypeScript will watch vigilantly, yelling at us when we pass in parameters that don't match the types. 

But, it is only as strict as we want/need it to be.

Notably developers have found that simply porting their JavaScript codebase to TypeScript and gradually adding types reveals numerous bugs that just did not get caught. JavaScript alone will never check if you are passing in the expected parameters, it simply can never really know what to expect *without* type annotations.

Null safety and type safety are life savers as they are often at the root of many hard-to-trace bugs. They are a great way to address famous "billion dollar mistake" and save valuable time and money doing far more interesting things than tracing down what the heck propagating an `undefined` through the codebase. Like deploying to production,or planning yourself a nice siesta.

### I'm not convinced yet?

Providing type annotations doubly acts as beautiful documentation for code. Plus, you can opt-in as much as you want! Maybe you are just not ready to convert your older JavaScript code base entirely over to TypeScript. You can steadily port as much as you want over, because in the end, TypeScript just transpiles to JavaScript! Transpiling here meaning that, our source code (TypeScript) gets translated to a very similar language (JavaScript). You can also tell TypeScript to be as strict as you need to it to be. Of course, the more strict options you enable the more TypeScript can do for you!

TypeScript adds a lot of safety checks and quality of life features to VSCode for JavaScript. For example, it provides a lot of auto-completion for JavaScript in VSCode that just wasn't possible before.

In the end if that's still not convincing and you just refuse to look at the rest of this cookbook, that's OK. Maybe we can't be friends. But this hosted cookbook will always be left open for you to come back.
