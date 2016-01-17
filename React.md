#React

React is a way of writing declarative views where a view is a function of data defined in terms of state in some point in time. 

##React Component

A React Component is a JavaScript function that owns a section of the DOM. Components can be composed together with other components to build up a UI. Composition is achieved through a tree structure with a component parent-child hierarchy (functions in functions). There can only be one root component and there can be many levels of children. Each component handles its own state and rendering its part of the DOM in a UI.

##JSX

JSX is an extension of JavaScript that allows you to use XML like constructs in your components to define your view structure. When you use a React JSX transformer, like Babel or TypeScipt, JSX elements are transformed to calls to plain JavaScript React.createElement methods. 

There are some difference from HTML that you have to watch out for while writing JSX. You can't use JavaScript keywords. For instance, instead of defining a `class` on your element you use `className`. Instead of using `for` to relate a label with an element use `htmlFor`.

When you want to render HTML elements you use lowercase as first letter of JSX element. When you want to render a React Component use uppercase as first letter of the JSX element.

You could write your views with React.createElement instead of JSX, but JSX is more approachable by designers and developers that aren't comfortable with JavaScript. Also, using React components with JSX we move from writing views with imperative hard to understand HTML template logic to writing declarative functions to manage the state of components with JavaScript.

##React Component Rendering

One of the great benefits of using React is the speed at which it can update the DOM.

Components have a method named setState. You called this method any time the state for the component is updated. setState will mark the component as dirty and determines if it needs to re-render the virtual DOM for itself and all of the child components in its sub-tree. At the end of each event loop React will call render on all of the components that have been marked dirty by setState, so rendering is a batched operation.

If you want to improve performance of rendering you should define shouldComponentUpdate to short circuit rendering on components that should not re-render based on comparison of previous and next props and state. If you use immutable data structures, like [immutable.js](https://facebook.github.io/immutable-js/), for props and state, this comparison becomes trivial since you don't have to do deep comparison of immutable objects.

If React determines that it should render it will render a virtual DOM. React compares the virtual DOM from the previous render with the current render to decide what has to actually be updated in the device's DOM. To help React efficiently compare DOM versions you can give child elements a `key` attribute with a value that is unique among its siblings. It is much more efficient for React to update the virtual DOM made of JavaScript objects than say a browser DOM. If changes are found React will [reconcile](https://facebook.github.io/react/docs/reconciliation.html) the DOM to make the minimum number of changes to the device's DOM.

##React Component Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. Related to the section on "React Component Communication and Data Flow Rules", there are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about, I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###React Component Props

You can think of props as the components configuration or the external public API of the component. Once props are set don't expect them to stay in sync as the component goes through its lifecycle. Within the component, props should be considered immutable or read only. After the component is constructed props should not be changed. Parent components can pass props to their child components. A component can set default values for props to keep default values consistent when a props aren't passed in. Default props values will be overridden if props are passed into the component.

###React Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. A parent can pass in props to set default values for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to re-render the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.

##React Component Communication and Data Flow Rules

React components communicate by passing data. To make it easier to work with components we impose rules that govern how components communicate. The rules can be ignored because there are no mechanisms in React to enforce them, but adhering to some rules makes your application easier to understand, maintain, and debug.

###Handling State: MVC vs React

React is not MVC, it is a view engine. It is hard to compare it to something like Angular, Ember or Backbone because they have fundamental differences. Data in React should be handled differently than it is in MVC. 

In MVC you have mutable data or state in the form of models. Multiple views can depend on a model. Changing a model could change multiple views. A view could change multiple models. In large applications this many-to-many relationship can make it difficult to understand what views depend on models and what models are updated from views. Understanding how data flows and the state of the applicaton over time can be a challenge. The nature of the model view relationship also leads to funky circular dependencies that can be ripe with hard to find bugs.

In React data or state is mutable, but it is private, not shared, and managed in components. Because state is internal there are no side effects from shared state like MVC. When you keep the number of stateful components low, it is easier to understand the state of your application over time, hence easier to debug and maintain. When we know where, when, and how state changes and that properties don't change once they are set there is a lot of  guess work removed when we are trying to debug or update our applications. By using the concept of stateful and stateless components you can get a better handle on what the current state of your application is.

###Stateful Components

Stateful components manage the state for itself and its child components. A stateful component would map its state to props that are passed down to stateless components for consumption. The props don't change once set and will always be consistent with the stateful component that set them. Whenever state is updated (calling `this.setState`) the stateful component will render itself and all of its children.

###Stateless Component

Stateless components don't hold state and depend on thier stateful parent component for state. The stateless component can trigger events that would cause the stateful component to update state and therefore update the stateless component. 

###Data Flow

Another way to look at the stateful-to-stateless component relationship is in terms of data flow. Data is passed down the React component hierarchy in props (parent-to-child communication). Props should be immutable because they are passed down every time higher components are re-rendered, so any changes in props would be loss on each re-render. So, changing props after they are set is a good way to introduce bugs if you want them, but why would you? 

You can enforce props to be read-only in TypeScript by using a TypeScript property with only a getter or in JavaScript ES5+ with `object.defineProperty` with `writable` set to `false`. Defining props with persisted immutable data structure, like those found in [immutable.js](https://facebook.github.io/immutable-js/) help further in enforcing immutability and helps simplify comparisons when you need to compare changes in props.

###Event Flow

Events flow up the hierarchy and can be used to instruct higher components to update state (child-to-parent communication). When you have components that need to communicate that don't share a parent child relationship, you can write a global event system or even better use a pattern such as [Flux](https://facebook.github.io/flux/) to enable cross component communication.

These flow rules allows us to easily visualize how data streams through our application. 

##React Component Lifecycle

