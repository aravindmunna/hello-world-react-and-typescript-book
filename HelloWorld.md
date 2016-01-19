#Hello World

We will review a few code samples. You can download the code samples with the link provided for each sample. You can also get a copy of the repository for the samples by cloning

https://github.com/charleslbryant/hello-world-react-and-typescript.git

##Requirements

The code samples have a couple requirements

- npm package manager (Node 5.2.0 used in development)
- IE 10+ or similar modern browser that supports the History API

##Installing Samples

To install a sample you need to run npm to get the required dependencies. You should open a console at the root path of the sample directory and run

`npm install`

Then

`tsd install`

These commands will install the required npm packages and TypeScript Typings.

##Run the Sample

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
If you don't understand this I recommend that you find an introduction to HTML course online.

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
```


DOM.render(<Main />, root); 

- Split component into two components
- Manage component state with prop and state objects
- Handle events
- Compose input form from components to accept user input to update state

Wishful thinking for a more robust scalable crazy Hello World app:

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