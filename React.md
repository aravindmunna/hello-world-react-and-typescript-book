#Writing React Components with TypeScript

##React

React is a way of writing declarative views where a view is a function of data defined in terms of state.

##React Component

A React Component is a reusable object that can be composed together with other component to render a user interface (UI). Composition is through a parent child hierarchy. There can only be one root component and there can be many levels of children to build up the DOM for a UI. To make it easy we impose rules that govern how components communicate (the rules can be ignored).

Data is passed down the component hierarchy in properties (parent-to-child communication) and properties should be immutable because they are passed down every time higher components are re-rendered, so any changes in properties would be loss on each re-render. So, changing properties after they are set is a good way to introduce bugs if you want them.

Events flow up the hierarchy and can be used to instruct higher components to update state (child-to-parent communication). When you have components that need to communicate that don't share a parent child relationship, you can write a global event system or use a pattern such as Flux to enable cross component communication.

Components manage their own state, but every component doesn't need state that persists across rendering. When state is updated the application is re-rendered. When you keep the number of stateful components low, it is easier to understand the state of your application over time, hence easier to debug and maintain. When we know where, when, and how state changes and that properties don't change once they are set there is a lot of guess work removed when we are trying to debug or update our applications.

These flow rules allows us to easily see how data streams through our application. We move from writing imperative hard to understand transactional logic to writing declarative functions to manage the state of components.

##React Component Rendering

One of the great benefits of using React is the speed at which it can update the DOM.

Components have a method named setState. You called this method any time the state for the component is updated. setState will mark the component as dirty and determines if it needs to re-render the virtual DOM for itself and all of the child components in its sub-tree. At the end of each event loop React will call render on all of the components that have been marked dirty by setState, so rendering is a batched operation. 

If you want to improve performance of rendering you should define shouldComponentUpdate to short circuit rendering on components that should not re-render based on comparison of previous and next props and state. If you use immutable data structures, like [immutable.js](https://facebook.github.io/immutable-js/), for props and state, this comparison becomes trivial since you don't have to do deep comparison of immutable objects.

If React determines that it should render it will render a virtual DOM. React compares the virtual DOM from the previous render with the current render to decide what has to actually be updated in the device's DOM. It is much more efficient for React to update the virtual DOM made of JavaScript objects than say a browser DOM. If changes are found React will [reconcile](https://facebook.github.io/react/docs/reconciliation.html) the DOM to make the minimum number of changes to the device's DOM.

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

##TypeScript React Component

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


##React Component Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. There are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about, I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###React Component Props

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

###React Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. A parent can pass in props to set default vaules for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to rerender the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.