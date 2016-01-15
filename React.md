#React

##Props and State

Props and state are both plain JavaScript objects that can be passed to a component to provide the attributes of a component used to alter the behavior of the component and render HTML. There are some suggestions regarding props and state to help keep your application maintainable and easier to debug. These are only suggestions and there is nothing in React that will prevent you from not following them. If you want your application to be easier to reason about I suggest you follow some guidelines to help make the state in your application easier to maintain and debug.

###Component Props

You can think of props as the external public API of the component. Once props is set don't expect it to stay in sync as the component goes through its lifecycle. Within the component, props should be considered immutable or read only. After the component is constructed props should not be changed. Parent components can pass props to their child components. A component can set default values for props to keep default values consistent when a props aren't passed in. Default props values will be overridden if props are passed into the component.  

###Component State

You can think of state as the private members of the component. State is only accessible within the component, the parent cannot mutate state. A parent can pass in props to set default vaules for state during construction of the component. Components use state to enable interactivity. As event happen in the UI state is updated and this causes the React to rerender the DOM with the new state. So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.