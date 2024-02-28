# Diving Deeper: Understanding Types of Code Generation Tools

## Template-based Code Generation

### How Template-based Code Generation Works

Template-based code generators operate on the principle of templates, which are predefined patterns or structures. These Templates serve as blueprints for generating code.

Developers create template with placeholders for variables or dynamic content. When the code generator is invoked, the template is rendered with the provided variables to generate the final code.

### Code Generation Tools

#### PlopJS

<!-- Brief overview of PlopJS -->

[Plop.js]() is a powerful and flexible code generator that simplifies the process of creating and maintaining code templates. It provides a command-line interface and a declarative syntax for defining templates and generating code based on them.

##### Key Features

1. **Code Generation:** Plop.js enables developers to generate code snippets, files, and entire project structures with ease. It offers a simple and intuitive command-line interface, allowing users to define custom templates and automate repetitive coding tasks.
2. **Template-driven Approach:** Plop.js follows a template-driven approach, where developers can define templates using Handlebars, a popular templating language. This allows for dynamic content generation, making it easy to customize generated code based on specific requirements.
3. **Interactive Prompts:** Plop.js provides interactive prompts during code generation, allowing developers to input custom values or make choices based on predefined options. This feature enhances flexibility and ensures that generated code aligns with specific project needs.
4. **Customizable Actions:** Plop.js allows developers to define custom actions that can be executed during code generation. These actions can include file manipulation, installing dependencies, running scripts, or any other custom logic required for the project setup.

##### Putting PlopsJS into Action

1. **Setting up PlopJS**

**Installation**

```sh
yarn add --dev plop
```

**Create a `Plopfile.js` at the root of your project**

```typescript
export default function (plop) {
  // create your generators here
  plop.setGenerator("basics", {
    description: "this is a skeleton plopfile",
    prompts: [], // array of inquirer prompts
    actions: [], // array of actions
  });
}
```

**Create a plopfile with the desired generator**

```typescript
export default function (plop) {
  // controller generator
  plop.setGenerator("controller", {
    description: "application controller logic",
    prompts: [
      {
        type: "input",
        name: "name",
        message: "controller name please",
      },
    ],
    actions: [
      {
        type: "add",
        path: "src/{{name}}.js",
        templateFile: "plop-templates/controller.hbs",
      },
    ],
  });
}
```

In my experience with Plop.js, I often find the need for multiple templates, each involving various prompts and actions. Fortunately, Plop.js facilitates a declarative approach, enabling me to manage all these distinct plopfiles efficiently within a single Plopfile.js file.

**Example**

```typescript
import { NodePlopAPI } from "plop";

export default (plop: NodePlopAPI) => {
  plop.load("./actors/model.ts");
  plop.load("./actors/module.ts");
};
```

The `plop.load` method is used to load separate plopfiles. In this case, it loads two templates: `./actors/model.ts` and `./actors/module.ts`.

**High-Level Idea**

The idea here is to modularize the code generation process. Instead of defining all templates, prompts, and actions in a single massive file, you can break them down into smaller, more manageable files.

Each file, like `./actors/model.ts` and `./actors/module.ts`, contains the specific configuration for generating code related to models or modules.

2. **Defining Templates**

The syntax within a template file is a Handlebars template. It allows for the definition of dynamic content, conditional logic, and the application of case modifiers.

Let's consider a simple example template for generating a controller:

```typescript
// templates/controller.ts

import { ModuleController } from 'your-module-controller-library';
import { decodeRequest, generateSuccessResponse, generateErrorResponse, httpStatus } from 'your-utils-library';

export const {{camelCase actionName}}: ModuleController = async (request, h) => {
  try {
    const { payload, db } = decodeRequest(request);
    const {{camelCase moduleName}} = await db.models.{{pascalCase moduleName}}.{{actionName}}(payload);
    return generateSuccessResponse(h, {{camelCase moduleName}});
  } catch (error) {
    return generateErrorResponse(h, `Error {{actionVerb}} {{camelCase moduleName}}`, httpStatus.BAD_REQUEST);
  }
};
```

In this example template:

- `{{camelCase actionName}}` and `{{pascalCase moduleName}}` are placeholders for dynamic content. During code generation, these placeholders will be replaced with actual values provided by the user.

- `{{actionVerb}}` is another placeholder for a dynamic action verb. This could be customized based on user input during code generation.

- `camelCase` and `pascalCase` are called case modifiers, and they are used to convert the action name to camelCase and PascalCase, respectively. You can create your own case modifier if needed (more on this in the next section).

3. **Creating Generators**

Plop.js generators are essentially sets of instructions for creating or modifying files and directories based on user input. In your provided code, the generator is named 'model'. Let's break down the key concepts:

1. Setting Up Helpers

In previous sections, I mention about customizing a case modifier. In this section, I'll cover the concept of 'helper' and how it can be used to customize the generated code. Take the following example:

```typescript
plop.setHelper('upperCase', str => str.toUpperCase());
plop.setHelper('singularCase', str => inflection.singularize(str));
plop.setHelper('printModelName', modelName => inflection.singularize(inflection.camelize(modelName)));
```

- **Helpers**: Helpers are functions that modify or manipulate data during the code generation process. In this example, three custom helpers are defined: `upperCase`, `singularCase`, and `printModelName`. These helpers perform tasks like converting strings to uppercase, singularizing, and camelizing model names.

2. Defining Generators

```typescript
plop.setGenerator('model', {
  description: 'Create a model',
  prompts: async (inquirer: Inquirer) => { /* ... */ },
  actions: answers => { /* ... */ },
});
```


- **Generator Definition:** The `setGenerator` method defines a generator named 'model'. This generator is responsible for creating a Sequelize model.

- **Description:** A description is provided to explain the purpose of the generator.

- **Prompts:** The `prompts` property defines the questions to be asked during the code generation process. It uses the `Inquirer` library to prompt the user for information needed to generate the model.

- **Actions:** The `actions` property defines the steps to be taken after the user provides the necessary input. These steps include appending constants to a file and adding a new model file.


3. Prompting for Model Information

```typescript
prompts: async (inquirer: Inquirer) => { /* ... */ }
```

- **Prompt Function:** The `prompts` property is a function that uses the `inquirer` library to prompt the user for information needed to generate the model.

- **Recursive Prompts:** The prompts include asking for the model name and, through a recursive function (`addColumnsPrompt`), dynamically adding columns based on user input.

4. Generating Actions

```typescript
actions: answers => { /* ... */ }
```

- **Actions Function:** The `actions` property is a function that specifies the actions to be taken based on the answers provided by the user. Some built-in actions such as:
    - **add** add new files to the project
    - **append** append content to files
    - ...(Read more [here](https://plopjs.com/documentation/#built-in-actions))

- **File Operations:** The actions include appending a constant to a file and adding a new model file based on a Handlebars template (`model.hbs`).

## Read More

- [Auto Generate Code](https://docs.google.com/presentation/d/1nVApi5bklwTIYIctGVKlx7bQGaMgPWLfR7AdALZ3m-8/edit#slide=id.g142d6c309a9_0_28) *from **Thai Hoa***
- [Real world project](https://github.com/loctvl842/hapi-sequelize-seed) *from **loctvl842** aka **ON NO***
