#Components in Separate Files

This sample demonstrates how to split your components into multiple files. It is still just the simple display Hello World, but now it is modular and reusable.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.3

The HTML is still exactly the same as sample #2. 

##src/main.tsx

We only have two changes to explore the main.tsx file.

```javascript
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';
import * as DOM from 'react-dom';
import HelloWorld from './helloworld';//Add new import

const root = document.getElementById('app');

class Main extends React.Component<any, any> {
    constructor(props: any){
        super(props);
    }

	public render() {
		return (
          <HelloWorld />//Return our new HelloWorld component
        );
	}
}

DOM.render(<Main />, root); 
```

Let's look at the differences.

---

```
import HelloWorld from './helloworld';
```

We added a new import to import the HelloWorld component.Notice that we use a relative path to the component with no extension.

---

```
return (
    <HelloWorld />
);
```

Instead of returning the markup directly, we are returning the HelloWorld component and delegating the rendering of the markup to this new component. By having the markup in a separate component we can add `<HelloWorld />` multiple times and only have to change it in one place. We have a single point of truth for what the markup for Hello World should be and we can reuse it.

To use external components they have to be in scope, which is why we imported it. Another point to make is that a React component needs to have only one root element. If we were to add more markup or components here, we would have to wrap it in a parent element. 

Example:

```
return (
    <div>
        <HelloWorld />
        <HelloWorld />
    </div>
);
```

This would display "Hello World" twice. Notice how we started the component name with an uppercase letter, React expects components to start with uppercase and DOM elements to start with lowercase.

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
    }

	public render() {
		return (
          <div>Hello World!</div>
        );
	}
}
```

If you paid attention to the previous sample you will notice that this is almost exactly like the old main.tsx. We copied main.tsx to a new file called helloworld.tsx. We gave the component class a new name and we deleted the import for DOM. We don't need to import DOM because we don't need to call anything from React.DOM.

That was pretty easy so I don't think that we need to review this code.