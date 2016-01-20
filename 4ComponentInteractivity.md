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










