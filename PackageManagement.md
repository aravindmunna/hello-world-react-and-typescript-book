# PackageManagement

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