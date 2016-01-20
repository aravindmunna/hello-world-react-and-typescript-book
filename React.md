#React

*React is a library for building composable user interfaces. It encourages the creation of reusable UI components which present data that changes over time.*

*Pete Hunt - http://facebook.github.io/react/blog/2013/06/05/why-react.html*

Tom Occhino and Jordan WalkeReact announced React as an open source project at JSConfUS 2013 - https://www.youtube.com/watch?v=GW0rj4sNH2w. Pete Hunt followed up in a talk he gave at JSConfEU and JSConfAsia. There were a lot of nay sayers as a result of the announcement and Pete titled his talk after a sarcastic Twitter post, "React: Rethink Best Practices." This talk goes into the design decisions behind React and gives solid reasons for using it - https://www.youtube.com/watch?v=DgVS-zXgMTk&feature=iv&src_vid=x7cQ3mrcKaY&annotation_id=annotation_3048311263.

For you functional minded folks, another way to think of React is as a way of writing declarative views where a view is a function of data defined in terms of the state of data in some point in time. 

```
v = view
p = props
s = state
v = f(p,s)
```

React is a small library with a simple API. It is not an MVC framework. You can think of React as the view in MVC. It just renders views and enables interactivity by responding to events.  

Facebook and Instagram developed React and actually use it in production on their very large scale applications and development teams. You can write isomorphic applications by rendering views on both the client and server with the same JavaScript code. With the announcement of React Native you can render views on any device (IOS, Android...). Netflix is using React to render UIs on televisions! Its hard to deny React as a viable option for application development.

I think React has delivered on redefining best practices for efficiently developing and rendering views.

##React Component

A React Component is a JavaScript function that owns a section of the DOM. Components can be composed together with other components to build up a UI. If you build components properly they become like Legos for UIs. Small, focused on a specific concern, easy to reuse and repurpose them to build UIs to solve various problems.

Composition is achieved through a tree structure with a component parent-child hierarchy. You can think of compositions as functions in functions or components in components with the hierarchy modeled as a DAG. There can only be one root component, but there can be many levels of children. Each component handles its own state and rendering its part of the DOM in a UI.

##JSX

React doesn't have a template system, but you can use markup that feels like HTML by using JSX. JSX is an extension of JavaScript that allows you to use XML like constructs in your components to define your view structure. 

JSX is not a template it is a high level representation of the DOM that must be transformed before it is used. When you use a JSX transformer, like Babel or TypeScipt, JSX elements are transformed to plain JavaScript `React.createElement` methods. 

There are some difference from HTML that you have to watch out for while writing JSX. You can't use JavaScript keywords. For instance, instead of defining a CSS `class` attribute on your JSX element you use `className`. Instead of using `for` to relate a label with an element you use `htmlFor`.

When you want to render HTML elements you use lowercase as first letter of JSX element. 

```html
<div className="container">
    <label htmlFor="hello">Hello</label>
    <input type="text" id="hello" />
</div>
```

When you want to render a React Component use uppercase as first letter of the JSX element.

```html
<HelloWorldComponent displayName="Charles" />
```

You could write your views with `React.createElement` instead of JSX, but JSX is more approachable by designers and developers that aren't comfortable with JavaScript. 

Writing React components with JSX we move from writing views with imperative hard to understand HTML template logic to writing declarative functions to manage the state of components with JavaScript. It's a win-win. Developers can write logic with a proper development language and not a wierd template abstraction and Designers still get the comfort of working with HTML like structures.

##React Component Rendering

One of the great benefits of using React is the speed at which it can update the DOM. Speed is a core value of the team that develops React. They achieved speed by relying on a virtual representation of the DOM and some clever algorithms that allows React to make changes to the DOM faster than traditional view engines.

Components have a method named `setState`. You call this method any time the state for the component is updated. `setState` will mark the component as dirty to help React optimize what actually needs to be re-rendered. When a component is marked for rendering the children in its sub-tree will also be rendered. At the end of each event loop React will call render on all of the components that have been marked dirty by `setState`. This means that during a single cycle React can render multiple components in one batch.

If you want to improve performance of rendering you can define `shouldComponentUpdate` to short circuit rendering on components that should not re-render based on comparison of previous and next props and state. If you use immutable data structures, like [immutable.js](https://facebook.github.io/immutable-js/), for props and state, this comparison becomes trivial since you don't have to do deep comparison of immutable objects. If `shouldComponentUpdate` returns false, the component and its sub-tree will not be re-rendered. This is very help when a child is not dependent on its parent for state.

If React determines that it should render it will render a virtual DOM. React compares the virtual DOM from the previous render with the current render to decide what has been updated in the DOM. When comparing the old and new DOM React compares nodes in the DOM tree. If it finds a node that doesn't have the same element or component type, it will remove the old one including its entire sub-tree and replace it with the new one. This provides a big gain in terms of performance because React doesn't have to waste time tyring to reconcile sub-trees if the parent nodes don't match.

To help React efficiently compare nodes you should assign child elements a `key` attribute with a value that is unique among its siblings elements. This makes it easier for React to compare elements. React compares the attributes of elements to determine if there was a change. If a change was found, only the attributes that have changed are updated.

It is much more efficient for React to work with the virtual DOM made of JavaScript objects than say a browser DOM. You can read more about the heuristics React uses to reconcile the DOM to make the minimum number of changes to a device's DOM

[https://facebook.github.io/react/docs/reconciliation.html](https://facebook.github.io/react/docs/reconciliation.html).

##React Component Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. There are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about, I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###React Component Props

You can think of props as the components configuration or the external public API of the component. Props are the external parameters that are passed into a component. Once props are set don't expect them to stay in sync as the component goes through its lifecycle. Within the component, props should be considered immutable or read only. After the component is constructed props should not be changed. Parent components can pass props to their child components. A component can set default values for props to keep default values consistent when a props aren't passed in. Default props values will be overridden if props are passed into the component.

###React Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. State is an internal member of a component. A parent can pass in props to set default values for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to re-render the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.

##React Component Communication and Data Flow Rules

React components communicate by passing data. To make it easier to work with components we impose rules that govern how components communicate. The rules can be ignored because there are no mechanisms in React to enforce them, but adhering to some rules makes your application easier to understand, maintain, and debug.

###Handling State: MVC vs React

React is not MVC, it is a view engine. It is hard to compare it to something like Angular, Ember or Backbone because they have fundamental differences. Because of these differences you should handle data in React differently than you do in MVC. 

In MVC you have mutable data or state in the form of models. Multiple views can depend on a model. Changing a model could change multiple views. A view could change multiple models. In large applications this many-to-many relationship can make it difficult to understand what views depend on models and what models are updated from views. Understanding how data flows and the state of the application over time can be a challenge. The nature of the model view relationship also leads to funky circular dependencies that can be ripe with hard to find bugs (like the well known issues with Global state).

In React data or state is mutable, but it is private, not shared, and managed in components. Because state is internal there are no side effects from shared state like MVC. When you keep the number of stateful components low, it is easier to understand the state of your application over time, hence easier to debug and maintain. We can use stateful components to pass immutable properties to stateless components that will always be consistent with the stateful component. When we know where, when, and how state changes and that properties don't change once they are set there is a lot of  guess work removed when we are trying to debug or update our applications. By using the concept of stateful and stateless components you can get a better handle on the state of your application at any point in time.

###Stateful Components

A stateful component is a container that provides data or state service to stateless components. They manage the state for itself and its child components. If we compare it to DDD, a stateful component would be like an aggregate for a bounded context. 

A stateful component maps its state to props that are passed down to stateless components for consumption. The props are immutable and don't change once set and will always be consistent with the stateful component that set them. Whenever state is updated (calling `this.setState`) the stateful component will render itself and all of its children, so children stay consistent across re-renders. 

A stateful component should handle events triggered by its children. Ideally, stateful component wouldn't have props and would be able to compose its state on all its own, independent of any parent or ancestors. It should be detached from its parent in terms of rendering. It should only re-render when it has new state, not when it's parent re-renders. It could possible use the `shouldComponentUpdate` to prevent re-rendering by parent.

```javascript
shouldComponenUpdate(nextProps) { 
    return false; 
    //or return reference equality of immutable props
    //return this.props === nextProps;
}
```

This is one way to prevent wasteful propagation of unnecessary renders.

For clarity I would recommend naming stateful components with a "Container" suffix so that it is evident that it is a stateful component (e.g. `MyFunkyContainer`, `MyFunkyTestContainer`).

###Stateless Components

Stateless components don't hold state and depend on their stateful parent component container for state. The stateless component can trigger events that would cause the stateful component to update state and therefore update the stateless component. Stateless components are reusable and they aren't dependent on a specific stateful component container, but requires a container to pass props. In fact, Jason Bonta of Facebook recommended having a mock or test container called an explorer with static data for the sole purpose of testing stateless components.

###Data Flow

Another way to look at the stateful-to-stateless component relationship is in terms of data flow. Data is passed down the React component hierarchy in props (parent-to-child communication). Props should be immutable because they are passed down every time higher components are re-rendered, so any changes in props would be loss on each re-render. So, changing props after they are set is a good way to introduce bugs if you want them, but why would you? 

You can enforce props to be read-only in TypeScript by using a TypeScript property with only a getter or in JavaScript ES5+ with `object.defineProperty` with `writable` set to `false`. Defining props with persisted immutable data structure, like those found in [immutable.js](https://facebook.github.io/immutable-js/) help further in enforcing immutability and helps simplify comparisons when you need to compare changes in props.

###Event Flow

Events flow up the hierarchy and can be used to instruct higher components to update state (child-to-parent communication). When you have components that need to communicate that don't share a parent child relationship, you can write a global event system or even better use a pattern such as [Flux](https://facebook.github.io/flux/) to enable cross component communication.

These flow rules allows us to easily visualize how data streams through our application. 

##Reference Equlity: The Holy Grail

##React Component Lifecycle

