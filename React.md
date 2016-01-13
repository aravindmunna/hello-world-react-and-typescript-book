#React

##Props and State

Props and state are both basic objects that can be passed to a component to provide the attributes of a component used to render HTML. 

Within the component, after props are set props are immutable. Parent components are responsible for passing props to their child componentents. A component can set a default value for props, but default values will be overriden if props are passed into the component.

State is only updateable by the component, the parent cannot mutate state. Parent components can pass in state to their child components, but this is only to set default values. Components are able to update their state which is how interactivity is achieved.

So, state should only be used for a component's attributes that need to change over time. All other attributes should be props.