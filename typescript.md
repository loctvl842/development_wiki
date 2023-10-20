# Typescript

## Set up

1 **Install Node.js and npm:**

2. **Create a new project directory:**

```sh
mkdir my-ts-node-app
cd my-ts-node-app
```

3. **Initialize a new Node.js Project:**

```sh
yarn init
```

4. **Install Typescript:**
```sh
yarn add -D typescript
```

5. **Create a Typescript config file:**

```sh
yarn tsc --init
```

Sample config file:
```jsonc
{
  "compilerOptions": {
    "lib": ["es5", "es6"],
    "target": "es6" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "module": "CommonJS" /* Specify what module code is generated. */,
    "moduleResolution": "node",
    "baseUrl": "./" /* Specify the base directory to resolve non-relative module names. */,
    "paths": {
      "@src/*": [
        "src/*"
      ] /* Have to config in package.json use with module-alias library */
    } /* Specify a set of entries that re-map imports to additional lookup locations. */,
    "sourceMap": true /* Create source map files for emitted JavaScript files. */,
    "outDir": "dist" /* Specify an output folder for all emitted files. */,
    "removeComments": true /* Disable emitting comments. */,
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,
    "strict": true /* Enable all strict type-checking options. */,
    "strictPropertyInitialization": false,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */,
    "resolveJsonModule": true
  },
  "files": ["app.ts"],
  "include": ["src/**/*.ts", "app.ts"],
  "exclude": ["node_modules", "dist", "__tests__"]
}
```

6. **Install Node.js Type Definitions:**

```sh
yarn add -D @types/node
```
