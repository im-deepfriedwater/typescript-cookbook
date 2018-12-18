### Help me install please...

Sure! It's quite easy. Most commonly, people install TypeScript as a Node.js package to compile (or more specifically _transpile_) from the command-line. Be sure to have a reasonably recent installation of Node.js!

If you are beginning your first TypeScript project, go into a directory where you will be working with TypeScript. 

### From your favorite terminal, do the following:

If you do not already have a package.json file
`npm init` 

Then go through the all the configuration options.

Now, we do:
`npm install typescript`

To compile a TypeScript file:
`tsc cookingsomethinggood.ts`

### That's it!
Note when you compile a file TypeScript automatically creates a `built` folder with corresponding JavaScript files and folders. Don't forget that in the end TypeScript compiles to JavaScript. We can change the output directory and various other settings by modifying a file called `tsconfig.json`, but don't worry we'll get to that in the next section! Additionally, TypeScript works well with modern editors *particularly* VSCode.

### "Wait this page is so SHORT." 

Yup. Leveraging modern development workflows tends to do that to development environment installations.