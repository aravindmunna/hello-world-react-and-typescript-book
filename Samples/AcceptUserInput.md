# 6 - Accept User Input

This sample gives an basic example of accepting user input.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.6

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
    }
    
    public handleOnChange(event: any) : void {
        this.setState({ name: event.target.value });
    }

	public render() {
		return (
            <div>
                <div>
                    <input 
                        onChange={ e => this.handleOnChange(e) }
                    />
                </div>
                <div>
                    Hello { this.state.name }!
                </div>
            </div>
        );
	}
}

```

The significant changes here are we removed the button and `handleOnClick` method. We also added a text box and a `handleOnChange` method to handle changes as a user adds input to the text box.

----

```
public handleOnChange(event: any) : void {
    this.setState({ name: event.target.value });
}
```

This is a new method to handle the onChange event. In the method we are calling `this.setState` and updating the value of `name` in `this.state` to the value of the target element passed in the event.

---

```
public render() {
	return (
        <div>
            <div>
                <input 
                    onChange={ e => this.handleOnChange(e) }
                />
            </div>
            <div>
                Hello { this.state.name }!
            </div>
        </div>
    );
}
```

Here we added an input element and set its default value to `this.props.name`. We also bound the text box's onChange event to `handleOnChange` method.

---

With these changes we have implemented a unidirectional data flow. 

1. When the user enters something in the text box it triggers the `onChange` event. 
2. The `onChange` event is handled by the `handleOnChange` method.
3. The `handleOnChange` method updates the value of `name` in `this.state` and triggers a re-render of the component with `this.setState`.
4. `this.setState` ends in a call to the `render` method that updates the name in our "Hello" message. 

### Unidirectional Data Flow (UDF)

event > event handler > state > render 

Components are representations of the state of a view over time. As events are triggered over time they update state and re-render the component with the new state. The flow can be seen as a stream of events that flow in one direction that eventually update component state causing a component to re-render. If you know about CQRS, event streaming, or stream processing, there are similar concepts in each. UDF is a redundant theme in learning React, hence a redundant theme in this book.

The sample is a simple naive example because we aren't dealing with external or persisted data. The scope of the example makes it a little hard to understand UDF. If you are having trouble understanding, when you learn about Flux it will makes more sense. The Flux architecture helps you visualize data flow in a circular one way round trip. Even though it may be hard to see UDF within the context of a single controller the same event flow is used to accomplish UDF in React whether within a single component, a Flux architecture, or using Relay (another Facebook library). 

### UDF vs Bi-directional Data Flow

The important thing to notice about UDF is that we aren't attempting to create a complex bi-directional view binding with some external model. We are binding to the state modeled within a component with no dependency or complex mapping to some external data model. The component is responsible for expressing its own state, updating its state, passing properties to its child components, and re-rendering itself and children when state changes as the result of some event.

If we were to use a bi-directional binding to an external model, we would not know why state is being updated. Any number of views could have the same binding to the same model. With bi-directional binding a change to a view or model could cause updates to multiple models or views and it becomes hard to understand the data flow, especially when you are trying to solve a Sev1 incident.

If you'd like to know more about UDF, there is a lot about it online that can be found with a simple search. Actually, the original MVC pattern is an example of UDF. It was distorted when it moved to web clients. If you'd like to dig into the theory behind React and UDF, you can  research Functional Reactive Programming - https://en.wikipedia.org/wiki/Reactive_programming.











