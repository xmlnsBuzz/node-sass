# node-sass

npm에서 sass를 설치하고 사용하는 방법

Installing Bootstrap 4 via NPM
We're going to start our project right here at this stage. This will require using Node.js and its package manager to install bootstrap itself, along with a few other packages. This will give us access to Sass, live browser reloading, etc..

First, make sure you have Nodejs installed by opening up your console or command line:


> node -v


If this goes unrecognized, visit Nodejs.org and click downloads. Download the appropriate installer based on your OS and follow through the installation process with the default options. 

Once complete, reload your console or command line and you will have access to thoe node command.

Let's create a folder for our project and hop into it:

> mkdir bs4 && cd bs4


Next, we're going to run npm init to create a package.json file, which simply stores our dependencies.


> npm init -y


(Note: The -y flag simply allows us to skip answering the various prompts and instead provides them with the defaults)

Next, we're going to use npm once again to install several different packages as development dependencies:

> npm install gulp browser-sync gulp-sass --save-dev



gulp is a javascript task runner. You'll see how it works shortly if you're new.
browser-sync automatically refreshes our browser for us upon file changes.
gulp-sass enables sass compiling with our project.
Then, we're going to use npm one final time to install several packages as regular project dependencies:

*** 붙스트랩, 제이쿼리, 파퍼를 동시에 인스톨한다. 따로 해도 되지만 각 모듈의 일관성을 유지하기 위해 이렇게 하는 게 좋을 것 같다.

> npm install bootstrap jquery popper.js --save

bootstrap of course is the bootstrap package.
jquery is used by bootstrap.
popper.js is also used by bootstrap. It allows for positioning of popovers, tooltips, etc..
Next, open up your code editor. If you're using Visual Studio Code, simply type code . in the command line within the project folder and it will launch the app.

Inside of here, let's create the following folders: 

/src
    /assets
    /css
    /js
    /scss
Inside of /src, also create the 4 folders proceeding it as shown above.

Next, create an index.html file inside of /src/ with the following contents:

<!DOCTYPE html>
<html class="no-js" lang="en">
    <head>
        <title>Bootstrap 4 Layout</title>
        <meta http-equiv="x-ua-compatible" content="ie=edge">
	    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

        <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Raleway:400,800">
        <link rel='stylesheet' href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
        <link rel="stylesheet" href="/css/bootstrap.css">
        <link rel="stylesheet" href="/css/styles.css">
    </head>

    <body>
        <script src="/js/jquery.min.js"></script>
        <script src="/js/popper.min.js"></script>
        <script src="/js/bootstrap.min.js"></script>
    </body>
</html>
So, we have a few things happening here. I'm importing the Raleway font along with FontAwesome for icons. Then I'm referencing bootstrap.css itself and a styles.css file. These don't exist yet, but they will shortly.

Then, finally, we have our javascript files. They don't exist yet either. Hang tight though.

Let's also create a styles.scss file inside of the /src/scss/ folder. We're going to put in a quick variable and a ruleset to ensure that Sass compiling is working in a bit:

$bg-color: red; 

body {
    background: $bg-color;
}
In the root folder (the project folder), create a file called gulpfile.js and paste the following contents:

var gulp        = require('gulp');
var browserSync = require('browser-sync').create();
var sass        = require('gulp-sass');

// Compile sass into CSS & auto-inject into browsers
gulp.task('sass', function() {
    return gulp.src(['node_modules/bootstrap/scss/bootstrap.scss', 'src/scss/*.scss'])
        .pipe(sass())
        .pipe(gulp.dest("src/css"))
        .pipe(browserSync.stream());
});

// Move the javascript files into our /src/js folder
gulp.task('js', function() {
    return gulp.src(['node_modules/bootstrap/dist/js/bootstrap.min.js', 'node_modules/jquery/dist/jquery.min.js', 'node_modules/popper.js/dist/umd/popper.min.js'])
        .pipe(gulp.dest("src/js"))
        .pipe(browserSync.stream());
});

// Static Server + watching scss/html files
gulp.task('serve', ['sass'], function() {

    browserSync.init({
        server: "./src"  
    });

    gulp.watch(['node_modules/bootstrap/scss/bootstrap.scss', 'src/scss/*.scss'], ['sass']);
    gulp.watch("src/*.html").on('change', browserSync.reload);
});

gulp.task('default', ['js','serve']);
I will describe what's happening here based on the tasks defined above:

default task - When we type gulp in the command line, this is telling it to run both the js and serve tasks.
js task - This is simply specifying 3 different javascript files that are stored in the node_modules folder, which is created when we ran npm install ..., and moving them into our /src/js folder. This way, we're able to include them in our HTML file above by referencing /src/js instead of the node_modules folder.
serve task - The serve task launches a simple server and watches our sass files, and if any are changed, it calls the sass task. It also calls browser-sync when any * .html file is saved.
sass task - This takes both of the bootstrap sass files and our custom sass files and compiles them into regular CSS, and stores those CSS files into our /src/css folder.
Whew, that's a lot of work, right? Well, fortunately, we're done with the setup.

Let's run gulp in the command line:

> gulp
With any luck, http://localhost:3000 will load up in the browser and your background will be bright stinking red! This means everything should be good to go.
