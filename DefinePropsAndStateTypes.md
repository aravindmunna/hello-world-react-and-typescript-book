#Define Props and State Types

This sample gives shows how we can get more type checking to protect from errors by defining the expected type for Props and State.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.8

##src/interfaces.d.ts

```
interface IHelloFormProps {
	name: string;
	handleChange(event: any): void;
}

interface IHelloContentProps {
	name: string;
}

```

First order of business is to define the new types. Since we aren't passing in state to any components we won't define types for state objects, but its the same concept.

We are defining the types in a type definition file (file extention d.ts). This is the same type of file that we get from DefinitelyTyped. This is a scaled down example, but you can also use the same technique to build your own type definitions for external modules that don't have a current definition. 

---

```
interface IHelloFormProps {
```

`interface` is TypeScripts declaration for an interface. It is a way to declare a contract for the shape of a type. Shape in this sense is a collection of methods and properties along with their associated types. When you create objects of the type defined by the interface it is expected that the object will have the same shape. If it doesn't, TypeScript will give an error at compile time. If your IDE supports it, you can also get design time errors, code completion, etc.

These types are not used in the JavaScript that is generated, it is just used for type checking by the TypeScript compiler.

---

```
name: string;
handleChange(event: any): void;
```

Here we are defining the expected shape of objects that extend `IHelloFormProps`. Objects are expected to have a property named `name` of type `string` and a method named `handleChange` that accepts an argument named `event` of type `any` and has a return type `void`. 

Hopefully, you can understand the `IHelloContentProps` interface from the explanation of `IHelloFormProps`.

##src/helloform.tsx
```
/// <reference path="../typings/tsd.d.ts" />
/// <reference path="./interfaces.d.ts" />

import * as React from 'react';

export default class HelloForm extends React.Component<IHelloFormProps, any> {
    constructor(props: IHelloFormProps) {
        super(props);
    }

	public render() {
		return (
            <div>
                <input 
                    value={ this.props.name }
                    onChange={ e => this.props.handleChange(e) }
                />
            </div>
        );
	}
}
```

The `HelloWorld` component is updated to use the new types, `IHelloFormProps` and `IHelloContentContent`.

---

```
/// <reference path="./interfaces.d.ts" />
```

Here we are adding a reference to the new type definition file we created to hold our type interfaces.

---

```
export default class HelloForm extends React.Component<IHelloFormProps, any> {
```

In our class declaration we are saying that we want to extend `React.Component` using objects of type `IHelloFormProps`.

---

```
constructor(props: IHelloFormProps) {
```

In the constructor we update the `props` argument to be of type `IHelloFormProps`.

##src/hellocontent.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloContent extends React.Component<any, any> {
    constructor(props: any){
        super(props);
    }

	public render() {
		return (
            <div>
                Hello { this.props.name }!
            </div>
        );
	}
}
```
This is very similar to the change we made to `HelloWorldForm` component.




