#Development Environment

The first thing we need to do is setup a development environment to enable React and TypeScript development.

##TypeScript Development

####TypeScript Definition Files

To get ready for TypeScript development we need typings for React. An easy way to pull in typings is with tsd, a package manager to search and install TypeScript definition files. 

`npm install -g tsd`

You can install TypeScript typings with tsd. When you install your typings are saved in tsd.json. You can create a tsd.json file by running

`tsd init`

To install a typing, for example react, you can run tsd

`tsd install react --save`

You can find a lot of TypeScript definition at [https://github.com/DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped).

To make it easier, in the source code there is a tsd.json that includes all of the typings necessary for the demo application. To install these typing just run

`tsd install`

This will get the typings and place them in the typings folder. In the typings folder there is a tsd.d.ts file that has all of the typing references bundled into one file.

###TypeScript Configuration

There are a lot of configurations that you can set for the TypeScript compiler. To help manage the settings you can create a tsconfig.json file to hold the configuration.

We take advantage of this file in the Gulp task to compile TypeScript. Actually, in our gulpfile we use the tsconfig in `tsc.createProject('tsconfig.json')` to create a project that we can use in our compile task.