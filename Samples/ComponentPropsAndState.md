#Component Props and State

This sample gives an basic example of using props and state in your components.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.4

##src/main.tsx

```javascript
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';
import * as DOM from 'react-dom';
import HelloWorld from './helloworld';

const root = document.getElementById('app');

class Main extends React.Component<any, any> {
    constructor(props: any){
        super(props);
    }

	public render() {
		return (
            <div>
              <HelloWorld defaultName='World' />
            </div>
        );
	}
}

DOM.render(<Main />, root);  
```

The change to main.tsx is minor. Here is the one change.

---

```
return (
    <div>
        <HelloWorld defaultName='World' />
    </div>
);
```

We are just passing `defaultName` to the `HelloWorld` component. This is doing exactly what you think it is, its setting the default value for who we are saying Hello to. Notice that this name is explicit in defining this input. It is a default value, the HelloWorld component can change this value and the assumption is that it may be changed.

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
    }

	public render() {
		return (
            <div>
                Hello { this.state.name }!
            </div>
        );
	}
}
```

We made two changes to helloworld.tsx, but they are significant to the interactivity of our little application.

----

```
constructor(props: any){
    super(props);
    this.state = { name: this.props.defaultName };
}
```

In the constructor we are setting the initial state for this component. `this.state` is a plain JavaScript object. It is a mutable representation of the model for the view. `this.state` is expected to change over time as events occur in the component. In this sample we are initializing `this.state` with an object literal `{ name: this.props.defaultName }`.

`this.props.defaultName` was passed in from main.tsx. `this.props` is also a plain JavaScript object. The difference being it is an immutable representation data used by the component. `this.props` should not be changed and they are passed in by parent components. If you change props, they will be overwritten when the parent rerenders. Mutating props puts your component in an inconsisten state, so don't do it.

---

```
return (
    <div>
        Hello { this.state.name }!
    </div>
);
```
The next change is to the return value of the `render` method. We are using `this.state.name` to set the name dispayed in the UI.

In this sample we use `this.props` to initialize `this.state`. We know that we want to update the name of who we say hello to, so we are using a state object to represent the name. If we didn't want to update name, we could use props directly. For example

```
return (
    <div>
        Hello { this.props.name }!
    </div>
);
```
Using props like this is saying that we don't expect this component to change the name. In fact, to make it explicit, we changed the name from `defaultName` to `name`. Also, `main.txt` would have to be changed from passing `defaultName` to `name`. Using props means we expect the parent to be in control of changing the value of props. Using state means we expect the component to be in control of its own state.




