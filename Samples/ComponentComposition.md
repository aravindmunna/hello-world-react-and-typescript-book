#Component Composition

This sample gives a basic example of refactoring the Hello World application by composing the UI with modular components.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.7

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';
import HelloForm from 'helloform';
import HelloContent from 'hellocontent';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
        this.handleChange = this.handleChange.bind(this)
    }
    
    public handleChange(event: any) : void {
        
        this.setState({ name: event.target.value });
    }

	public render() {
		return (
            <div>
                <HelloForm 
                    name = { this.state.name }
                    handleChange = { this.handleChange } 
                />
                <HelloContent 
                    name = { this.state.name }
                />
            </div>
        );
	}
}

```

The `HelloWorld` component is updated to compose the same UI as the previous example using two modular components, `HelloForm` and `HelloContent`.

----

```
import * as React from 'react';
import HelloForm from 'helloform';
import HelloContent from 'hellocontent';
```

To compose with components the components you want to compose have to be in scope. To do this we import the components. `HelloForm` and `HelloContent` are imported by using the `import` statement with the `from` value being the name of the component.

---

```
public render() {
	return (
        <div>
            <HelloForm 
                name = { this.state.name }
                handleChange = { this.handleChange } 
            />
            <HelloContent 
                name = { this.state.name }
            />
        </div>
    );
}
```

The actual composition occurs in the `render` method. We add the `HelloForm` and `HelloContent` components. Notice that the name of the components start with uppercase to differentiate them from HTML elements. Each of the components accept some properties and we pass them with a syntax that is similar to defining HTML attributes.

---

By composing UIs in this manner we move from an imperative style of building UIs to a declarative one. Instead of defining every element and attribute we want to use in the UI we delegate the definition to a modular reusable component. 

Since the modular components are reusable we can compose multiple UIs with them. We can compose with components developed by other teams. We get to focus on the unique aspects of our domain and delegate other lower level concerns to modular components.

##src/helloform.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloForm extends React.Component<any, any> {
    constructor(props: any){
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
Here we have a new component responsible for the collecting user input for our tiny application. By now this code should be familiar. We just moved the input element from the `HelloWorld` component and encapsulated in this new component. 

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
This is a component responsible for displaying the hello message. Again, this component is a result of encapsulating this section of the DOM out of the `HelloWorld` component.

###Stateless Components

One interesting observation about these components is that they are stateless. THey does not hold any state and they rely on a parent component to pass `props` to it in order to do its work. This includes event handlers and data as you can see in the code. 

When possible using stateless components are preferred in React. They help to improve performance because they lessen the amount of processing that React has to do with the component.

