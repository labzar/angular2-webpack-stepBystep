# angular2-webpack-stepBystep

# Table of Contents
# File Structure
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
## Common configuration
## Development configuration
## Production configuration
## Test configuration

## QUICKSTART
The QuickStart application has the structure of a real-world Angular application and displays the simple message: Hellooo World :)
**Prerequisite: Install Node.js and npm**

If Node.js and npm aren't already on your machine, install them. Our examples require node v4.x.x or higher and npm 3.x.x or higher. To check which version you are using, run node -v and npm -v in a terminal window.

**Step 1: Create and configure the project**
