# 2 - Components in Separate Files

This sample demonstrates how to split your components into multiple files. It is still just the simple display Hello World, but now it is modular and reusable.

The HTML is still exactly the same as sample #1. 

##src/main.tsx

We will only highlight and explore the two changes to the main.tsx file.

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

We added a new import to import the HelloWorld component. You will notice that this import is a little different. Before we only imported external modules. TypeScript knows to look in the node_moduels folder for these modules (you can define other locations, but we won't go into that here).

With our new custom internal module, we need to tell TypeScript where it is located. That's why we have a relative path to the module as the `from` value.

---

```
return (
    <HelloWorld />
);
```

Instead of returning the markup we are returning the HelloWorld component and delegating the rendering of the markup to the component. Now, we can add `<HelloWorld />` multiple times and only have to change it in one place. We have a single point of truth for what the markup for Hello World should be and we can reuse it.

Notice how we started the component name with a uppercase letter, React expects components to start with uppercase and DOM elements to start with lowercase.

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