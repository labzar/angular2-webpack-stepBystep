# angular2-webpack-stepBystep

# Table of Contents
# File Structure
```
angular2-webpack-stepBystep/
 ├──config/                    * our configuration
 |   ├──helpers.js             * helper functions for our configuration files
 |   ├──karma-test-shim.js     * tells Karma what files to pre-load
 |   ├──karma.conf.js          * karma config for our unit tests
 |   ├──webpack.common.js      * contains the common configuration between development, production, and test environments
 │   ├──webpack.dev.js         * our development webpack config
 │   ├──webpack.prod.js        * our production webpack config
 │   └──webpack.test.js        * our testing webpack config
 │
 ├──src/                       * our source files that will be compiled to javascript
 |   ├──main.ts        * our entry file for our browser environment
 │   │
 |   ├──vendor.ts      * our vendor file
 |   |
 |   ├──polyfills.ts           * our polyfills file
 │   │
 |   ├──index.html             * Index.html: where we generate our index page
 │   │
 │   │
 │   ├──app/                       * WebApp: folder
 │   │   ├──app.component.css      * a simple css of components in app.component.ts
 │   │   ├──app.component.html     * a simple html of components in app.component.ts
 │   │   ├──app.component.ts       * a simple version of our App component
 │   │   ├──app.component.spec.ts  * a simple test of components in app.component.ts
 │   │   └──app.module.ts          * app.module.ts: a simple version of our App module
 │   │
 │   └──public/                * static stuf are served here
 │       ├──css/              
 │       └──image/  
 │
 │
 ├──tsconfig.json              * config that webpack uses for typescript
 ├──package.json               * what npm uses to manage it's dependencies
 ├──karma.conf.js              * contains a shortcut to our karma.conf.js located in config folder
 └──webpack.config.js          * contains a shortcut to our webpack.dev.js located in config folder
 

```
# What is the difference between Angular1 and Angular2?
I pointed here some major difference between angular 1 and angular 2 that every one should know.

First thing, Angular 2 is not the upgrade of angular 1. Angular 2 is completely rewritten.
- Angular 2 is using Typescript which is super set of javascript.
- Angular 1.x was not built with mobile support in mind, where Angular 2 is mobile oriented.
- Angular 1 core concept was $scope, and you will not find $scope in angular 2.0. Angular 2 is using zone.js to detect changes.
- Performance improved in Angular 2.0 as compared to Angular 1.x. Bootstrap is now platform specific in angular 2. So if application is bootstrap from browser it will call different bootstrap as compare to mobile app.
- Angular 1.x controllers are gone. We can say that controllers are replaced with “Components” in Angular 2.
![alt tag](https://qph.ec.quoracdn.net/main-qimg-c9beed549d710c65b99fe8799ef5123f?convert_to_webp=true)

# What is Webpack?
Webpack takes modules with dependencies and generates static assets representing those modules :shipit:
![alt tag](http://webpack.github.io/assets/what-is-webpack.png)

## Why webpack?
webpack is usually compared to tools like Make, Grunt, Gulp, Browserify or Brunch. However, some of these tools (Make, Grunt, and Gulp, which are task runners) have a much different purpose than webpack, which is a module bundler. Comparing them directly could lead to sorts of confusion, so let’s first draw a distinction between these types of tools.

## What Are Task Runners?
Task runners literally make it easier to handle tasks, such as linting, building, or developing your project. Compared to bundlers like Browserify, Brunch, or webpack, they have a higher level focus. Bundlers, on the other hand, have a much more specific goal.

## What Are Bundlers?
Roughly put bundlers take assets, such as JavaScript files in, and then transform them into format that's suitable for the browser of the end user to consume. This process of bundling happens to be one of the most important problems in web development and solving it well you can remove a large part of pain from the process.

Bundlers can work in tandem with task runners. You can still benefit from their higher level tooling while leaving the problem of bundling to more specialized tools.

# Configuring Webpack
Begin by setting up the development environment.

Create a new **project folder**

COPY CODE
```
mkdir angular-webpack-stepBystep
``` 
``` 
cd angular-webpack-stepBystep
```
Add these files to the root directory (you can take it from our repository):
- package.json
- tsconfig.json
- webpack.config.js
- karma.conf.js
- config/helpers.js

## Common configuration
Webpack is a NodeJS-based tool so its configuration is a JavaScript commonjs module file that begins with require statements as such files do.

**webpack.common.js** will contains the common configuration between development, production, and test environments.
if you take a look inside webpack.common.js you will see three principale parts:
First we specify the entry:
```
entry: {
  'polyfills': './src/polyfills.ts',
  'vendor': './src/vendor.ts',
  'app': './src/main.ts'
},
```
we are splitting our application into three bundles:
- **polyfills** - the standard polyfills we require to run Angular applications in most modern browsers.
- **vendor** - the vendor files we need: Angular, lodash, bootstrap.css...
- **app** - our application code.

Next we specify the loaders:
```
module: {
    loaders: [
      {
        test: /\.ts$/,
        loaders: ['awesome-typescript-loader', 'angular2-template-loader']
      },
      {
        test: /\.html$/,
        loader: 'html'
      },
      ...
      }
```
Finally we add two plugins:
```
plugins: [
  new webpack.optimize.CommonsChunkPlugin({
    name: ['app', 'vendor', 'polyfills']
  }),

  new HtmlWebpackPlugin({
    template: 'src/index.html'
  })
]
```
**COMMONSCHUNKPLUGIN:**
Webpack can separate the App code from vendor code with CommonsChunkPlugin
**HTMLWEBPACKPLUGIN:**
Webpack can inject scripts and links for us with the HtmlWebpackPlugin.

## Development configuration
The development build relies on the Webpack development server which we configure near the bottom of the file.

Although we tell Webpack to put output bundles in the dist folder, the dev server keeps all bundles in memory; it doesn't write them to disk. So we won't find any files in the dist folder (at least not any generated from this development build).
```
output: {
    path: helpers.root('dist'),
    publicPath: 'http://localhost:8080/',
    filename: '[name].js',
    chunkFilename: '[id].chunk.js'
  },
```
Grab the app code at the end of this guide and try:

COPY CODE
```
npm start
```

## Production configuration
Configuration of a production build resembles development configuration ... with a few key changes.

```
output: {
    path: helpers.root('dist'),
    publicPath: '/',
    filename: '[name].[hash].js',
    chunkFilename: '[id].[hash].chunk.js'
  },
```
and we add additional plugins:

**NoErrorsPlugin** - stops the build if there is any error.
**DedupePlugin** - detects identical (and nearly identical) files and removes them from the output.
**UglifyJsPlugin** - minifies the bundles.
**ExtractTextPlugin** - extracts embedded css as external files, adding cache-busting hash to the filename.
**DefinePlugin** - use to define environment variables that we can reference within our application.

Grab the app code at the end of this guide and try:

COPY CODE
```
npm run build
```

## Test configuration
The purpose of webpack.test.js is to runs and configures test.
we have three main files:
- config/webpack.test.js
- config/karma.conf.js
- config/karma-test-shim.js

Grab the app code at the end of this guide and try:

COPY CODE
```
npm test
```

# QUICKSTART
The QuickStart application has the structure of a real-world Angular application and displays the simple message: Hellooo World :)
**Prerequisite: Install Node.js and npm**

If Node.js and npm aren't already on your machine, install them. Our examples require node v4.x.x or higher and npm 3.x.x or higher. To check which version you are using, run node -v and npm -v in a terminal window.

## Step 1: Create and configure the project**
