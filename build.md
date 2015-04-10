# Building your project's front-end

We have a standard. Please stick to it.

Every CFPB project with a non-trivial front-end should:

 1. Use **Grunt** to accomplish all front-end build tasks.
 1. Use **Less** as a CSS pre-processor.
 1. Use an optional **config.json** file in the same directory as the gruntfile to store build variables (internal API endpoints, PII, etc.). This file should be consumed by a Grunt task and listed in `.gitignore`.
 1. Only require **two commands** to install dependencies and build all static assets:
   1. `npm install`
   1. `grunt build`
 1. These two commands (and any additional commands if there's a strong argument for them) should be stored in a frontendbuild.sh file to make things easier for our CI environments.

You should avoid checking dependencies and minified assets into source control. It's common practice to keep source files in a `src` directory and compiled/minified assets in a `dist` directory.

## Installing Node

Mac: `brew install node`

Linux:

```shell
yum-config-manager --enable epel
yum install nodejs -y
```
## Installing Grunt and Bower

```shell
npm install -g grunt-cli bower
```

## Pegging versions

It's good practice to specify specific versions in any dependency management system such as maven, pip, ivy, npm, bower, etc. Yes, it incurs a bit of management overhead in that you have to manually change version numbers when you want to upgrade a dependency. This extra work pays off in absence of time spent tracking down unexpected changes or errors due to a version upgrade of which you were unaware.

### Bad:

```javascript
"library": "~0.2.4",
```

### Good:

```javascript
"library": "0.2.4",
```

## Building JavaScript and Less

We use [Grunt](http://gruntjs.com/) to automate the compilation of JavaScript and Less files.

Here are some helpful plugins for this:

- [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) for concatinating and minifying JS
- [grunt-contrib-less](https://github.com/gruntjs/grunt-contrib-less) for compiling Less and CSS files
- [grunt-contrib-cssmin](https://github.com/gruntjs/grunt-contrib-cssmin) for minifying CSS
- [grunt-contrib-clean](https://github.com/gruntjs/grunt-contrib-clean) for cleaning folders
- [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch) for watching and compiling assets on the fly
- [grunt-browserify](https://github.com/jmreidy/grunt-browserify) for using Node style CommonJS modules clientside

## Errors you may encounter

1. `bower install` fails with `Unable to parse global .bowerrc file: Arguments to path.join must be strings`

I've seen this on Jenkins builds. To fix:

`HOME="" bower install`