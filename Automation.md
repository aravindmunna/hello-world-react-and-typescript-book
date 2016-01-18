#Automation

Next, we will setup our build automation. The primary reason we want to do this now is so we don't have to run the commnand line everytime we make changes. In fact, our build automation will allow us to automatically see the results of changes in the browser after we save them. We can start one simple task in the command line and the automation will take care of linting, compiling, bundling, refreshing the browser and more when ever we make changes.This allows us to get very fast feedback on changes. If we have fast unit tests, this automation will also help us keep some bugs from creeping into our app. So, build automation will not only make development go faster, provide faster feedback, help keep quality high, it will also give us a consistent way to build our application.

##Gulp
We are going to use Gulp 4 for this tutorial. At the time of this writing, this new version of Gulp hasn't been released. The main reason I decided to go with this early release version is that it has new features that will make our automation a little easier to work with.

The main feature we want to exploit is the new task execution system. Before Gulp 4, all tasks ran in parallel. This was great becuse it made things fast, but many times there is a series of tasks that need to run in sequence. Getting tasks to run sequentially can get complicated. With the new task execution we can define a pipeline of tasks and decide if we want parallel or series execution.

First we need to install Gulp 4. Because I already have Gulp installed I need to firt uninstall Gulp and then reinstall version 4.

`npm uninstall --save-dev gulp`

`npm install --save-dev "gulpjs/gulp#4.0"`

`npm install --save-dev "gulpjs/gulp-cli#4.0"`

If you have worked with Gulp before, you'll notice that I installed gulp and gulp-cli separate. The CLI was moved to a separate module to make the core install lighter for use cases that don't need the CLI.

###Vinyl

Just some background on vinyl because it is interesting to me. Gulp uses [vinyl](https://github.com/gulpjs/vinyl), a virtual file format, which is why we don't need to create temp files to pass through automation workflows like Grunt. Files are converted to vinyl (`gulp.src()`) and can be piped to different gulp middleware as they flow through gulp tasks and finally converted back to a physical file (`gulp.dest()`). Under the hood gulp uses the vinyl-fs module to work with files on a local file system, but the "interesting to me" part of vinyl is that modules can be created to use vinyl to work with any file source, DropBox, S3, Azure Storage... I haven't had a need to connect to external files, but it was exciting to realize that I can come up with new use cases to transform my build automation process with vinyl modules.

