# 2 - Basic React Component

This is a very basic example of a single React component. We could make it even simpler, but this example is still basic enough to start with.

**Source Code** 

https://github.com/charleslbryant/hello-react-and-typescript/releases/tag/0.0.2

##src/index.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Hello World</title>
        <link rel="stylesheet" href="css/bundle.css"/>
    </head>
    <body>
        <div id="app"></div>
        <script src="scripts/bundle.js"></script>
    </body>
</html>
```
If you don't understand this, I recommend that you find an introduction to HTML course online.

The important part of this file is `<div id="app"></div>`. We will tell React to inject its HTML output to this div.

##src/main.tsx

```javascript
/// <reference path="../typings/tsd.d.ts" />
import * as React from 'react';
import * as DOM from 'react-dom';

const root = document.getElementById('app');

class Main extends React.Component<any, any> {
	constructor(props: any) {
		super(props);
	}

	public render() {
		return (
			<div>Hello World</div>
		);
	}
}

DOM.render(<Main />, root); 
```
If you know JavaScript, this may not look like your mama's JavaScript. At the time I wrote this ES6 was only 6 months old and this sample is written with ES6. If you haven't been following the exciting changes in JavaScript, a lot of the syntax may be new to you.

This is a JavaScript file with a .tsx extention. The .tsx extenstion let's the TypeScript compiler know that this file needs to be compiled to JavaScript and contains React JSX. For the most part, this is just standard ES6 JavaScript. One of the good things about TypeScript is we can write our code with ES6/7 and compile it as ES5 or older JavaScript for older browsers. 

**Let's break this code down line by line.**

---

```javascript
/// <reference path="../typings/tsd.d.ts" />
```

The very top of this file is a JavaScript comment. This is actually how we define references for TypeScript typing files. This let's TypeScript know where to find the definitions for types used in the code. I am using a global definition file that points to all of the actual definitions. This reduces the number of references I need to list in my file to one.

---

```javascript
import * as React from 'react';
import * as DOM from 'react-dom';
```

The `import` statements are new in JavaScript ES6. They define the modules that need to be imported so they can be used in the code.

---

```javascript
const root = document.getElementById('app');
```

`const` is one of the new ES6 variable declaration that says that our variable is a constant or a read-only reference to a value. The `root` variable is set to the HTML element we want to output our React component to.

---

```javascript
class Main extends React.Component<any, any> {
```

`class` is a new declaration for an un-hoisted, stict mode, function defined with prototype-based inheritance. That's a lot of fancy words and if they mean nothing to you, its not important. Just knowing that `class` creates a JavaScript class is all you need to know or you can dig in at https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes.

`Main` is the name of the class. 

`extends` says that the `Main` class will be created as a child of the `React.Component<any, any>` class. 

`React.Component<any, any>` is a part of the TypeScript typing definition for React. This is saying that `React.Component` will allow the type `any` to be used for the components state and props objects (more on this later). `any` is a special TypeScript type that basically says allow any type to be used. If we wanted to restrict the props and state object to a specific type we would replace `any` with the type we are expecting. This looks like generics in C#.

---

```javascript
constructor(props: any) {
	super(props);
}
```

The `contructor` method is used to initialize the class. In our sample, we only call super(props) which calls the contructor method on our parent class `React.Component` passing any props that were passed into our constructor. We are using TypeScript to define the props as `any` type.

In additon to calling `super` we could also initialize the state of the component, bind events, and other tasks to get our component ready for use.

---

```javascript
public render() {
	return (
		<div>Hello World</div>
	);
}
```

Maybe the most important method in a React component is the `render` method. This is where the DOM for our component is defined. In our sample we are using JSX. We will talk about JSX later, just understand that it is like HTML and outputs JavaScript. 

JSX is not a requirement. You could use plain JavaScript. We can rewrite this section of code as

```javascript
public render() {
	return (
		React.createElement("div", null, "Hello World")
	);
}
```

The `render` method returns the DOM for the component. After the `return` statement you put your JSX. In our sample, this is just a simple div that says Hello World. It is good practice to wrap your return expression in parenthesis.

---

```javascript
DOM.render(<Main />, root); 
```

Lastly, we call `DOM.render` and pass `Main`, the React component we just created, and `root`, the variable containing the element we want to output the component to.

---

That's it, pretty simple and something we can easily build upon.
