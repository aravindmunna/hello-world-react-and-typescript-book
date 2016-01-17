#React

React is a way of writing declarative views where a view is a function of data defined in terms of state in some point in time. 

##React Component

A React Component is a reusable JavaScript object that can be composed together with other components to build up a user interface (UI). Composition is achieved through a component parent child hierarchy. There can only be one root component and there can be many levels of children. Each component represent a part of a document object model (DOM) for a UI. 

##React Component Communication and Data Flow Rules

To make it easier to work with React components we impose rules that govern how components communicate. The rules can be ignored because there are no mechanisms in React to enforce them.

Data is passed down the component hierarchy in properties (parent-to-child communication). Properties should be immutable because they are passed down every time higher components are re-rendered, so any changes in properties would be loss on each re-render. So, changing properties after they are set is a good way to introduce bugs if you want them, but why would you.

Events flow up the hierarchy and can be used to instruct higher components to update state (child-to-parent communication). When you have components that need to communicate that don't share a parent child relationship, you can write a global event system or even better use a pattern such as [Flux](https://facebook.github.io/flux/) to enable cross component communication.

Components manage their own state, but every component doesn't need state that persists across rendering. When state is updated the application is re-rendered. When you keep the number of stateful components low, it is easier to understand the state of your application over time, hence easier to debug and maintain. When we know where, when, and how state changes and that properties don't change once they are set there is a lot of  guess work removed when we are trying to debug or update our applications.

These flow rules allows us to easily visualize how data streams through our application. 

##JSX

JSX is an extension of JavaScript that allows you to use XML like constructs in your components to define your view structure. When you use a React JSX transformer, like Babel or TypeScipt, JSX elements are transformed to calls to plain JavaScript React.createElement methods. 

There are some difference from HTML that you have to watch out for while writing JSX. You can't use JavaScript keywords. For instance, instead of defining a `class` on your element you use `className`. Instead of using `for` to relate a label with an element use `htmlFor`.

When you want to render HTML elements you use lowercase as first letter of JSX element. When you want to render a React Component use uppercase as first letter of the JSX element.

You could write your views with React.createElement instead of JSX, but JSX is more approachable by designers and developers that aren't comfortable with JavaScript. Also, using React components with JSX we move from writing views with imperative hard to understand HTML template logic to writing declarative functions to manage the state of components with JavaScript.

##React Component Rendering

One of the great benefits of using React is the speed at which it can update the DOM.

Components have a method named setState. You called this method any time the state for the component is updated. setState will mark the component as dirty and determines if it needs to re-render the virtual DOM for itself and all of the child components in its sub-tree. At the end of each event loop React will call render on all of the components that have been marked dirty by setState, so rendering is a batched operation. 

If you want to improve performance of rendering you should define shouldComponentUpdate to short circuit rendering on components that should not re-render based on comparison of previous and next props and state. If you use immutable data structures, like [immutable.js](https://facebook.github.io/immutable-js/), for props and state, this comparison becomes trivial since you don't have to do deep comparison of immutable objects.

If React determines that it should render it will render a virtual DOM. React compares the virtual DOM from the previous render with the current render to decide what has to actually be updated in the device's DOM. It is much more efficient for React to update the virtual DOM made of JavaScript objects than say a browser DOM. If changes are found React will [reconcile](https://facebook.github.io/react/docs/reconciliation.html) the DOM to make the minimum number of changes to the device's DOM.

##React Component Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. Related to the section on "React Component Communication and Data Flow Rules", there are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about, I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###React Component Props

You can think of props as the components configuration or the external public API of the component. Once props are set don't expect them to stay in sync as the component goes through its lifecycle. Within the component, props should be considered immutable or read only. After the component is constructed props should not be changed. Parent components can pass props to their child components. A component can set default values for props to keep default values consistent when a props aren't passed in. Default props values will be overridden if props are passed into the component.

###React Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. A parent can pass in props to set default values for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to re-render the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.