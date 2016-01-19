#Hello World

We will review a few code samples. You can download the code samples with the link provided for each sample. You can also get a copy of the repository for the samples by cloning

https://github.com/charleslbryant/hello-world-react-and-typescript.git

##Setting Up Samples

###Requirements

The code samples have a couple requirements

- npm package manager (Node 5.2.0 used in development)
- IE 10+ or similar modern browser that supports the History API

###Installing Samples

To install a sample you need to run npm to get the required dependencies. You should open a console at the root path of the sample directory and run

`npm install`

Then

`tsd install`

These commands will install the required npm packages and TypeScript Typings.

###Run the Sample

To run the samples, open a console at the sample directory and run

`gulp`

This will kick off the default gulp task to run all the magic and open the index.html page in a browser.

##Basic React Component

Below is a very basic example of a single React component. We could make it even simpler, but this example is still basic enough to start with.

**Source Code** 

https://github.com/charleslbryant/hello-world-react-and-typescript/releases/tag/0.0.2

###src/index.html

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

###src/main.tsx

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

The very top of this file is a JavaScript comment. This is actually how we define references for TypeScript typing files. This let's TypeScript know where to find the definitions for types used in the code.

The import statements are new in JavaScript ES6. They define the modules that are used in the code.

Then we have a line that starts with `const`. This is new ES6 variable declaration that says that our variable is a constant. This variable named root is just defining the HTML element we want to output our React component to.

Now we get into the meat of the code. We start with a class declaration, `class Main extends React.Component<any, any>`. This is also new to ES6. We are defining a JavaScript class named `Main`. We also extend our `Main` class with the class `React.Component<any, any>`. `React.Component<any, any>` is saying that we want to extend `React.Component` and allow any type to be used for the state and props objects (more on this later).

Next, we have contructor method that is used to initialize the class. In our sample, we only call super(props) which calls the contructor method on our parent class React.Component passing any props that were passed into our constructor. We are using TypeScript to define the props as `any` type.

Maybe the most important method in a React component is the `render` method. This is where the DOM for our component is defined. In our sample we are using JSX. We will talk about JSX later, just understand that it is like HTML and outputs JavaScript. 

The `render` method returns the DOM for the component. In the return statement you put your JSX. In our sample, this is just a simple div that says Hello World.

Lastly, we call `DOM.render` and pass `Main`, the React component we just created, and `root` the node in our HTML file that we want to output to.

That's it pretty simple and something we can easily build upon.


##Components in Separate Files

##Manage Component State with Prop and State Objects

##Handle Events

##Accept User Input

##Thoughts for More Robust Scalable App

- Flux
- Relay
- GraphQL
- Application Configuration Management
- Session Management
- Caching
- Security
  - Tenant Management
  - User Management
  - Role Management
  - Authentication
    - Cookie
    - Token
    - OAuth
  - Authorization