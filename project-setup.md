### Project setup
TypeScript projects normally are structured as conventional NodeJS projects. The `package.json` and `tsconfig.json` files should be in the root directory. 

Reminder: TypeScript must be installed via NPM! `npm install typescript`. You can also quickly generate a `tsconfig.json` file with default options by entering `tsc --init` in your favorite terminal at the desired folder of a TypeScript project.

Assume our `tsconfig.json` file has the following contents:
```JSON
{   
    "compileOnSave": true,
    
    "compilerOptions": {
        "outDir": "./built",
        "allowJs": false,
        "target": "es2016",
        "noImplicitAny": true,
        "strict": true,
        "noImplicitThis": true,
        "rootDir": "src" 
    },
}
```
The following represents the structure of what our root directory would be:

```
src   <- commonly code is kept in the `src` folder.
    /nested_directory
        /nestedfile.ts
    /helloworld.ts
    /projectexample.ts
package.json
tsconfig.json
built
    /nested_directory
        /nestedfile.js
    /helloworld.js
    /projectexample.js
```

If one does not specify an `outputDir` in the `tsconfig.json` file then TypeScript defaults to outputting files into the `built` folder at the root of the project. Note that TypeScript preservers the file structure of the `src` folder. 

Let's say there are test scripts we DON'T want TypeScript to try to compile for some reason.
```JSON
{   
    "compileOnSave": true,
    
    "compilerOptions": {
        "outDir": "./built",
        "allowJs": false,
        "target": "es2016",
        "noImplicitAny": true,
        "strict": true,
        "noImplicitThis": true,
        "rootDir": "src" 
    },

    "exclude": [
        "src/test/**/*"
    ]
}

```
src   <- commonly keep code 
    /nested_directory
        /nestedfile.ts
    /helloworld.ts
    /projectexample.ts
test
    /testrunner.js
package.json
tsconfig.json
built
    /nested_directory
        /nestedfile.js
    /helloworld.js
    /projectexample.js
```

### Working with node modules in TypeScript
It would be a shame to took advantage of the NodeJS module ecosystem we have access to. Many npm modules have been updated by their creators to include types for us to work with, and some have not been updated. But that's ok if they have not! Npm modules that have not been updated to include type annotations are still compatible. 

First, we will look at trying to use a built-in module from NodeJS to read and write to a simple file. 

1. We'll have to install the types documentation for the NodeJS modules. Run, `npm install @types/node --save-dev` at the root of your project via your favorite terminal.

2. Make sure in the `"compilerOptions"` in your `tsconfig.json` field contains the following value and field:
```JSON
{
    "compilerOptions": {
        ...
        "moduleResolution": "node"
        ...
    }
```

```TypeScript
// src/readFile.ts
import * as fs from 'fs';
console.log(fs.readFileSync('./readme.md'));
```

### Running files
TypeScript does its checks during compile time. So, we can compile it by running `tsc` in your favorite terminal. If the output directory does not already exist TypeScript will make one for you, and copy your folder structure as well. 

`CD` into the `built` folder and look for the name of your TypeScript file, but with a `.js` extension. Run a node command using that file as the parameter. `node built/src/readFile.ts`. 

It works similar for non-built-ins! Be sure to `npm install <module-name>` and TypeScript will suggest to you if there is extra type documentation recommended for downloaded for that specific module. If the types exist, the module and any code you have that uses will be checked by TypeScript to be type safe! Additionally, it even enables editors that work well with TypeScript to provide an autocomplete that would not have existed prior. 

### Creating your own modules
Just as common, users create their own modules for convenient reuse. Here is how you can write and import different TypeScript programs as modules:

```TypeScript
// Assume this file is named something like, `src/shape.ts`
// Export allows us to use this class in other files.
export class Shape {
    // something to note here is that the keyword public 
    // will automatically make a `this.number = number;` for you. 
    public Shape(public numberOfSides: number, rounded: boolean) {
        this.rounded = rounded;
    }
} 
```

```TypeScript
// Assume this file is named something like, `src/shapeUser.ts
// AND that `src/shape.ts` exists.
// We destructure the shape property from the export, then
// we make an alias for us so we can label it as `shapeClass`
export {Shape as shapeClass} from "src/shape.ts";
const circle = new Shape(5, rounded = false);
} 
```

There's a lot more to it, but this should be good starting point for users to get a sense of how TypeScript projects can be setup. 