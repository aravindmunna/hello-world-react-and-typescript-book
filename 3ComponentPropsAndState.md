# 3 - Component Props and State

This sample gives an basic example of using props and state in your components.

The HTML is still exactly the same as sample #1. 

##src/main.tsx

The change to main.tsx is minor.

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

Here is the one change.

---

```
return (
    <div>
        <HelloWorld defaultName='World' />
    </div>
);
```

We are just passing `defaultName` to the `HelloWorld` component. This is doing exactly what you think it is, its setting the default value for who we are saying Hello to.

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



