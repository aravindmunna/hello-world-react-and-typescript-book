#React

##Component

A React Component is a reusable object that can be composed together with other component to render a user interface. Composition is through a parent child hierarchy. There can only be one root component and there can be many levels of children to build up the DOM for the UI.

With TypeScript we extend the `React.Component<P,S>` class to create components. You also define the type used for props and state by passing the expected types as <P,S>. 

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


##Component Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. There are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about, I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###Component Props

You can think of props as the components configuration or the external public API of the component. Once props are set don't expect them to stay in sync as the component goes through its lifecycle. Within the component, props should be considered immutable or read only. After the component is constructed props should not be changed. Parent components can pass props to their child components. A component can set default values for props to keep default values consistent when a props aren't passed in. Default props values will be overridden if props are passed into the component. 

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

###Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. A parent can pass in props to set default vaules for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to rerender the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.