# 4 - Component Interactivity

This sample gives an basic example of using adding interactivity to a component.

##src/helloworld.tsx

```
/// <reference path="../typings/tsd.d.ts" />

import * as React from 'react';

export default class HelloWorld extends React.Component<any, any> {
    constructor(props: any){
        super(props);
        this.state = { name: this.props.defaultName };
    }
    
    public handleChange(event: any) : void {
        this.setState({ name: "Charles" });
    }

	public render() {
		return (
            <div>
                Hello { this.state.name }!
                <button 
                    name = "Update"
                    onClick = { e => this.handleOnClic(e) }
                >Update</button>
            </div>
        );
	}
}

```

To add interactivity we:

- added an element for a user to interact with
- created a method to handle an event triggered by user interaction
- bound the new element's onClick event to the new event handler method

----

```
public handleOnClick(event: any) : void {
    this.setState({ name: "Charles" });
}
```


```
return (
    <div>
        Hello { this.state.name }!
        <button 
            name = "Update"
            onClick = { e => this.handleOnClick(e) }
        >Update</button>
    </div>
);
```











