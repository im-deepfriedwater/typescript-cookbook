### Project setup
TypeScript projects normally are structured as conventional NodeJS projects. The `package.json` and `tsconfig.json` files should be in the root directory.

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

