#TypeScript

##TypeScript Definition File References

To use external modules in TypeScript you should provide a definition of the types used in the module. This will activate the IDE and the TypeScript compiler's ability to use these types to help catch errors.

To use TypeScript typings in your code you have to provide a reference to the definition files. This can be done with a reference

`/// <reference path="someExternalModule.d.ts" />`

If you modules are proper node modules, you should have to use a reference. If you have problems importing a module that has a type definition, try including a reference to the type definition.

I use tsd to import my type definitions. This creates a tsd.d.ts file that contains a reference to all of the typings I have installed. I usually use this file as a reference instead of individually listing each typing I want to reference.

`/// <reference path="../typings/tsd.d.ts" />`

This is a little of a maintenance issue because I am using relative paths and if I move the file in the path heirarchy I have to update the reference path. There are ways to get around this with a tsconfig file, but it is out of scope for this simple guide.

I have a lot of trouble with references and the internal module import system with TypeScript. Using the reference and the root tsd.d.ts file has allowed me to use ES6 style import with little friction, but it took me some searching to get over some nuances that aren't apparent.

##Importing Modules

To get the strong typing in TypeScript it has to know about the types you will be using. You can define your custom types as you build up your application, but if you are using third party modules like React, you have to give TypeScript some help. 

###Import External Modules

To import an external module, like a node module, you just use the name of the module instead of the path. TypeScript is smart enough to search in common locations for the module.

```javascript
import SomeModule from ('some-external-module'); //import the default module
import * SomeModule from ('some-external-module'); //import all exports from the module
import {someExportA, someExportB as SomeModuelB } from ('some-external-module'); //import specific moduels and aliasing one of them to a specific name.
```

###Import Internal Modules

To import an internal module that you exported you need to import from a relative path (you can use a tsconfig file to get around relative paths). For example, if I exported a module named `somemodule` in a root folder named js, then I would import it like so.

```javascript
import SomeModule from ('./js/somemodule'); //import the default module
import * SomeModule from ('./js/somemodule'); //import all exports from the module
import {someExportA, someExportB as SomeModuelB } from ('./js/somemodule'); //import specific moduels and aliasing one of them to a specific name.
```

You can read more about importing modules  at [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).

You can read more about tsconfig file at [https://github.com/TypeStrong/atom-typescript/blob/master/docs/tsconfig.md](https://github.com/TypeStrong/atom-typescript/blob/master/docs/tsconfig.md)

