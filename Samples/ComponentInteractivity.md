# 5 - Component Interactivity

This sample gives an basic example of using adding interactivity to a component.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.5

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
    }
    
    public handleChange(event: any) : void {
        this.setState({ name: "Charles" });
    }

	public render() {
		return (
            <div>
                Hello { this.state.name }!
                <button 
                    name = "Update"
                    onClick = { e => this.handleOnClic(e) }
                >Update</button>
            </div>
        );
	}
}

```

To add interactivity we:

- added an element for a user to interact with
- created a method to handle an event triggered by user interaction
- bound the new element's onClick event to the new event handler method

----

```
public handleOnClick(event: any) : void {
    this.setState({ name: "Charles" });
}
```

This is a new method to handle the click event. We expect callers of this method to send the event as an argument so we defined the argument as type `any` so we can accept any event. We could define a specific or generic event type and restrict the argument to that type. We also define the method as returning `void` or nothing.

In the method we are calling `this.setState` and updating the value of `name`. When we call `this.setState` it causes the component to re-render with the new state.

---

```
return (
    <div>
        Hello { this.state.name }!
        <button 
            name = "Update"
            onClick = { e => this.handleOnClick(e) }
        >Update</button>
    </div>
);
```

We are adding a new HTML element, button (remember that HTML elements start with lowercase letter). The interesting bit is that we are binding the element's `onClick` event to the `handleOnClick` method. We are using the new ES6 arrow function expression to simplify function context binding.

####Function Context Binding

In the end, React is just plain JavaScript. The React Team has resisted the urge to add a lot of magic and have made strides to remove magic to reduce coupling. Magic is the encapsulated work that is hidden behind abstractions. When a library does work behind the scenes without you knowing it, its magic. Reducing this hidden work helps to losen coupling and that is a good thing because it enables reuse. The React team believes our code shouldn't have a lot of coupling to React.  
  
One example of this, related to our use of the arrow function above, is function context binding. There was a time when React automatically bound functions to `this`. If you aren't a JavaScript developer there is a chance that you don't know that `this` in JavaScript is different from `this` in C# or Java or `self` in Ruby. 
  
In some strongly typed languages, `this` is bound to an object instance. In JavaScript `this` is bound to the function it is called in. This causes a lot of confusion, but this decoupling of this from their parent function allows us to reuse functions outside of the parent, powerful and dangerous at the same time. 

To get this function reuse we can define the context for a function and have `this` represent what we want it to. In our example the arrow function is assigning this from the class `HelloWorld` to the `handleOnClick` method. If we wanted, we could use this same method in say a `GoodByeWorld` class and the arrow function could set the context of the `handleOnClick` method.

Without the arrow function we would use JavaScripts `bind` method to achieve the same effect. The arrow function is just some syntactic sugar for

`onClick = { handleOnClick().bind(this) }`

You can get more on arrow functions here https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions.










