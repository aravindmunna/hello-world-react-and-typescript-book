#TypeScript and React

##TypeScript React Component

With TypeScript we extend the `React.Component<P,S>` class to create React components. You also define the type used for props and state by passing the expected types as <P,S>. 

Below we have an example where we are exporting a component. Since there can be more than one component in a namespace we mark our default component with the `default` keyword. Even if there are no other components, setting default makes it easier to import the component without having to define the default in the import statement.

Since the example doesn't use props or state and we haven't created types for them we pass in `any` to let the IDE and TypeScript compiler know that any type is acceptable for props and state. If we wanted to use props and state, we would create an interface to define the type we are expecting.

```javascript
export default class MyComponent extends React.Component<{}, {}> {
    public render() {
        return (
            <div>
                Hello World!
            </div>
        );
    }
}```

This is just a simple guide, so any deeper is out of scope. If you would like to get more info on React Components in TypeScript, I suggest taking a look at the type definition at [https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/react/react.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/react/react.d.ts). Unless the signature has changed, you can do a search for `type ReactInstance = Component<any, any> | Element;` to get a feel for how the type is constructed and what properties and methods are available.

##TypeScript React Component Props and State

Below we are using props to set a default value for a state value and then using the state value to render in the UI. When using TypeScript for React we define interfaces for the props and state objects. You can see this in the example, IMyComponentProps and IMyComponentState are interfaces that define the types these objects should contain. If your IDE support TypeScript it may be able to use these interfaces to give you visual feedback when you have the wrong types. The TypeScript compiler will also use the interfaces to do type checking. You can get a similar effect in normal React by using propTypes, but I enjoy how TypeScript maps to my C# OOP braint, it makes the component cleaner and easier to read in my opinion.

```javascript
interface IMyComponentProps {
    someDefaultValue: string
}

interface IMyComponentState {
    someValue: string
}

export default class MyComponent extends React.Component<IMyComponentProps, IMyComponentState> {
    constructor(props : IMyComponentProps) {
        super(props);
        this.state = { someValue: this.props.someDefaultValue };
    }

    public render() {
        return (
            <div>
                Value set as {this.state.someValue}
            </div>
        );
    }
}```

This example is assuming that someValue may get changed over time because it is in a state object (we aren't showing the code that would change the state to keep the code simple). If we know that someValue won't change during the components lifetime we would just use the props values directly in render (`Value set as {this.props.someDefaultValue}`). Since props are immutable and shouldn't change, we only move props to state in the constructor when we know the values will change.