# 4 - Component Interactivity

This sample gives an basic example of using adding interactivity to a component.

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
        this.handleChange = this.handleChange.bind(this)
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
                    onClick = { e => this.handleChange(e) }
                >Update</button>
            </div>
        );
	}
}

```

We made two changes to helloworld.tsx, but they are significant to the interactivity of our little application.

----

```
constructor(props: any){
    super(props);
    this.state = { name: this.props.defaultName };
}
```

In the constructor we are setting the initial state for this component. `this.state` is a plain JavaScript object. It is a mutable representation of the model for the view. `this.state` is expected to change over time as events occur in the component. In this sample we are initializing `this.state` with an object literal `{ name: this.props.defaultName }`.

`this.props.defaultName` was passed in from main.tsx. `this.props` is also a plain JavaScript object. The difference being it is an immutable representation data used by the component. `this.props` should not be changed and they are passed in by parent components. If you change props, they will be overwritten when the parent rerenders. Mutating props puts your component in an inconsisten state, so don't do it.

---

```
return (
    <div>
        Hello { this.state.name }!
    </div>
);
```
The next change is to the return value of the `render` method. We are using `this.state.name` to set the name dispayed in the UI.

In this sample we use `this.props` to initialize `this.state`. We know that we want to update the name of who we say hello to, so we are using a state object to represent the name. If we didn't want to update name, we could use props directly. For example

```
return (
    <div>
        Hello { this.props.name }!
    </div>
);
```
Using props like this is saying that we don't expect this component to change the name. In fact, to make it explicit, we changed the name from `defaultName` to `name`. Also, `main.txt` would have to be changed from passing `defaultName` to `name`. Using props means we expect the parent to be in control of changing the value of props. Using state means we expect the component to be in control of its own state.






In the end, React is just plain JavaScript. They have resisted the urge to add a lot of magic and have made strides to remove magic to reduce coupling to the magic. That was abstract, but I'm just saying that loose coupling is a good thing because it enable reuse. The React team believes our code should have a lot of coupling to React. 

One example of this is function context binding. There was a time when React automatically bound functions to `this`. If you aren't a JavaScript developer there is a chance that you don't know that `this` in JavaScript is different from `this` in C# or Java or `self` in Ruby.

In some strongly typed languages, `this` is bound to an object instance. In JavaScript `this` is bound to the function it is called in. This causes a lot of confusion, but this loose coupling allows us to reuse functions. We can define the context for a function and have `this` represent what we want it to.



