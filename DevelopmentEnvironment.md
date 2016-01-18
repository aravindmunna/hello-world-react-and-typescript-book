#Development Environment

The first thing we need to do is setup a development environment to enable React and TypeScript development.

##IDE

To code React applications with TypeScript you can use any text editor. To make things easier it may be worth it to get an integrated development environment (IDE) that has features and plug-ins specifically for JavaScript and TypeScript development.

Choosing an IDE is a religious experience so I won't start an ideological war by saying use this or that IDE. I would recommend looking at some of the popular ones active today:

- [Aptana - aptana.com](http://www.aptana.com/)
- [Atom - atom.io](https://atom.io/)
- [Brackets - brackets.io](http://brackets.io/)
- [Cloud9 - c9.io](https://c9.io/) - cloud based editor
- [Eclipse - eclipse.org](https://eclipse.org/downloads/)
- [Komodo - komodoide.com](http://komodoide.com/)
- [NetBeans - netbeans.org](https://netbeans.org/)
- [Sublime - sublimetext.com](http://www.sublimetext.com/)
- [Vim - vim.org](http://www.vim.org/)
- [Visual Studio Code - code.visualstudio.com](https://code.visualstudio.com/)
- [WebStorm - jetbrains.com/webstorm](https://www.jetbrains.com/webstorm/)

I am probably missing some that others would consider popular. This is just a tip of the IDE iceberg to give some ideas on what's available. Initially, the IDE you choose is not important. I can say that I enjoy Atom, Brackets, Cloud9, Sublime and Visual Studio Code. I won't vote for one over the other because they all work. Try a few IDE's and pick one that fits your development style without having to jump through hoops to do common tasks. If your goal is to get up to speed on React and TypeScript development, don't spend too much time trying to find and configure the perfect IDE.

##Package Management

There can be many dependencies that need to be managed to enable development with external libraries, frameworks, tools, and APIs. Since we are talking about a JavaScript project, we will use the popular Node Package Manager (NPM) to manage most of our dependencies. NPM is like NuGet or Maven, but for node modules. 

If you don't have npm, you'll have to [install it (https://docs.npmjs.com/getting-started/installing-node)](https://docs.npmjs.com/getting-started/installing-node). 

###Initialize NPM

To get going with npm create a directory for your project (name isn't important). Then open a command prompt at the new directory. Now initialie npm for the project, run

`npm init`

This starts a utility to help you setup your project for npm. I usually update the description, version and author info. The utility takes that data you enter and creates a package.json file. You can think of package.json like package.config for NuGet or pom.xml for Maven. This file will have a listing of all of the dependencies for the project.

###Initial Dependencies

We will be using a few dependencies that we will install with NPM. We are able to separate dependencies that are only needed for development from dependencies that are needed at runtime. This is useful when you want to distribute an application. User's of your app won't have to download your development dependencies if they don't need them.

####Runtime Dependencies

Here is a list of our initial runtime dependencies

- bootstratp - CSS styling.
- react - view engine.
- react-dom - react dom rendering module.
- react-router - routing.
- toastr - UI alerts and messaging.

To install them run

`npm install --save bootstrap react react-dom react-router toastr`

####Development Dependencies

Here is a list of our development dependencies

- browserify - bundle up JavaScript dependencies into a single file.
- browser-sync - refresh browser after changes.
- del - delete files.
- eslint - lint our JavaScript files.
- gulp - automate build and development workflow. This is like NAnt, Nake, Make and other task runners.
- gulp-concat - bundle css files.
- gulp-connect - run a webserver with LiveReload.
- gulp-eslint - run eslint in gulp tasks.
- gulp-typescript - run typescript compile in gulp tasks.
- gulp-sourcemaps - generate TypeScript sourcemaps.
- stream-combiner2 - turn a series of streams into a single stream to enable single error events.
- typescript - TypeScript dependencies.
- vinyl-source-stream - convert Browserify stream to vinyl stream used by Gulp.
- watchify - persistent browserify bundler to speed up builds.

To install them run

`npm install --save-dev browserify browser-sync del eslint gulp gulp-concat gulp-connect gulp-eslint gulp-typescript gulp-sourcemaps stream-combiner2 typescript vinyl-source-stream watchify`


##Build

Next, we will setup our build automation. The primary reason we want to do this now is so we don't have to run the commnand line everytime we make changes. In fact, our build automation will allow us to automatically see the results of changes in the browser after we save them. We can start one simple task in the command line and the automation will take care of linting, compiling, bundling, refreshing the browser and more when ever we make changes.This allows us to get very fast feedback on changes. If we have fast unit tests, this automation will also help us keep some bugs from creeping into our app. So, build automation will not only make development go faster, provide faster feedback, help keep quality high, it will also give us a consistent way to build our application.

###Gulp
We are going to use Gulp 4 for this tutorial. At the time of this writing, this new version of Gulp hasn't been released. The main reason I decided to go with this early release version is that it has new features that will make our automation a little easier to work with.

The main feature we want to exploit is the new task execution system. Before Gulp 4, all tasks ran in parallel. This was great becuse it made things fast, but many times there is a series of tasks that need to run in sequence. Getting tasks to run sequentially can get complicated. With the new task execution we can define a pipeline of tasks and decide if we want parallel or series execution.

First we need to install Gulp 4. Because I already have Gulp installed I need to firt uninstall Gulp and then reinstall version 4.

`npm uninstall --save-dev gulp`

`npm install --save-dev "gulpjs/gulp#4.0"`

`npm install --save-dev "gulpjs/gulp-cli#4.0"`

If you have worked with Gulp before, you'll notice that I installed gulp and gulp-cli separate. The CLI was moved to a separate module to make the core install lighter for use cases that don't need the CLI.

###Vinyl

Just some background on vinyl because it is interesting to me. Gulp uses [vinyl](https://github.com/gulpjs/vinyl), a virtual file format, which is why we don't need to create temp files to pass through automation workflows like Grunt. Files are converted to vinyl (`gulp.src()`) and can be piped to different gulp middleware as they flow through gulp tasks and finally converted back to a physical file (`gulp.dest()`). Under the hood gulp uses the vinyl-fs module to work with files on a local file system, but the "interesting to me" part of vinyl is that modules can be created to use vinyl to work with any file source, DropBox, S3, Azure Storage... I haven't had a need to connect to external files, but it was exciting to realize that I can come up with new use cases to transform my build automation process with vinyl modules.

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