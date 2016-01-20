# Setting Up Samples

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

This will kick off the default gulp task to run all the magic to compile TypeScript, bundle our dependencies and code into one bundle.js file, and move all of the files need to run the application to a dist folder, open the index.html page in a browser, and reload the site when changes are made to source files.

