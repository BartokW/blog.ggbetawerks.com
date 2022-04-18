---
title: Getting Started With Angular
date: '2022-01-20 20:10:00'
header:
 image: "/assets/images/angular.svg"
 teaser: "/assets/images/angular.svg"
tags:
- angular
- software-development
---
## What is Angular
*[ https://angular.io/guide/what-is-angular](https://angular.io/guide/what-is-angular)*

Angular is a development platform, built on TypeScript. As a platform, Angular includes:
- A component-based framework for building scalable web applications
- A collection of well-integrated libraries that cover a wide variety of features, including routing, forms management, client-server communication, and more
- A suite of developer tools to help you develop, build, test, and update your code

## What is TypeScript
*[https://www.typescriptlang.org/](https://www.typescriptlang.org/)*

TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale.
- TypeScript adds additional syntax to JavaScript to support a **tighter integration with your editor. Catch errors early in your editor**.
- TypeScript code converts to JavaScript, which **runs anywhere JavaScript runs:** In a browser, on Node.js or Deno and in your apps.
- TypeScript understands JavaScript and uses **type inference to give you great tooling** without additional code.

## Prerequisites
*[https://angular.io/guide/setup-local](https://angular.io/guide/setup-local)*

To install Angular on your local system, you need the following:
- Node.js - Either an active LTS or maintenance LTS version
- npm package manager

## Angular CLI
You use the Angular CLI to do pretty much everything, creating projects, generating code, testing, building, and deployment.

### Installing Globally
To install the Angular CLI globably, open a terminal window and run the following command:
```bash
npm install -g @angular/cli
```

### Creating an application
To create a starter application run the command `ng new` and provide a name for the application `my-app` as shown here:
```bash
ng new my-app
```
The `ng new` command will prompt you about features to include in the initial app. At the time of writing this, Angular 13 is the current version, and you will be prompted asking whether you want routing included or not, and what format for html styling you want to use (css, scss, sass, less).  The project will be automatically configured based on your choices. These choices can be changed later, but it's a manual process to update the project.

### Running the application
The Angular CLi includes a development server so you can build and serve your app locally.
You will need to navigate into the app folder `my-app` in this example, and then launch the server.
```bash
cd my-app
ng serve --open
```
Including the `--open` option will automatically open your browser to `http://localhost:4200/` the default index page for your app.

If the installation and setup was successful you should see a page like the following.
![Initial Demo App Starting Screen](/assets/images/angular-intro/angular-intro-initial-screen.png)

This server will stay running, and watch for changes to the code which will trigger a rebuild / refresh in the browser.

## Explore the Generated App
Open the `my-app` folder your preferred web development IDE, I'm using Visual Studio Code. The expanded folder structure will look something like this. 

![Screenshot of folder structure](/assets/images/angular-intro/angular-intro-file-list.png)

### Top Level
- **.angular** - New in Angular 13, and is used to store cache information to speed up compilation.
- **.vscode** - Holds some default configuration for Visual Studio Code
- **node_modules** - Folder where npm stores the installed packages
- **src** - The application source code lives here, more details later.
- **.browserslistrc** - Used by the build system to adjust the css and js output to support the specified browsers.
- **.editorconfig** - Used by your editor to maintain a consistent coding style
- **.gitignore** - A default configuration telling git which folders / files it can ignore.
- **angular.json** - The Angular workspace file. It defines all the projects within this workspace, and configures the different build targets. Such as serve, build, and test
- **karma.conf.js** - Configures the Karma test runner. By default at this time the project is set up to use the Jasmine testing library, with the Karma test runner
- **package.json** and **package-lock.json** - The npm package file, which defines the list of library dependencies. 
- **README.md** - A default readme file that provides list of valid Angular CLI commands that can be run
- **tsconfig.json** and **tsconfig.\*.json** - Provides configuration for the typescript transpiler. The app and spec versions of the file indicate settings specific to the application, or to the test (spec) runner

### src Folder
- **assets** - a place to store assets needed by your application, for example any images that are being used
- **environments** - A place to store some environment specific configuration with the environment.prod.ts file replacing the environment.ts when making a production build.
- **favicon.ico** - A default favicon that is used, feel free to replace with an appropriate one for your application
- **index.html** - This is the default web page that will be loaded when loading your application. Feel free to customize as needed, just remember the html tag `app-root` is where the angular application will appear when launched.
- **main.ts** - This is the launching point of the application. The default code here will enable production mode when compiled as a production build, and bootstraps the initial Angular module. In my experience, I've very rarely needed to modify this file.
- **polyfills.ts** - This is a place to add any polyfills that are needed for supporting older browsers.
- **styles.{css/scss/sass/less}** - This is where you place any application wide styles. The specific extension will depend on which styling you chose when creating the application.
- **test.ts** - This is the launching point when running the tests.
- **app** - The source code for the application, more detail below

### src/app Folder
- **app-routing.module.ts** - This will only exist if you chose to include routing when creating your application. Provides the scaffolding to add new routes to your application.
- **app.module.ts** - The initial module for the application that includes and bootstraps the components necessary to run the application
- **app.component.ts** - The initial component that is generated. This file contains the code necessary to make it work.
- **app.component.html** - This is the template for the app.component.ts file
- **app.component.{css/scss/sass/less}** - This is specific styling for the app.component. The specific extension will depend on which styling you chose when creating the application.
- **app-component.spec.ts** - This is the tests for the app.component.ts file

## Parts of the Application
### Module
dsfgdfg

### Components
Components are the main piece for building Angular applications.  You will likely build several components, and combine components into other more complex components when building your application.
Each component will have:
- A TypeScript class that defines the behaviour
- An HTML template to define what will be rendered
- A selector which defines the tag name to be used in templates, and for CSS styling

### Routing
gsdfgdsfg

### Pipes
Pipes are used to transform values for display in templates. These allow you to define a standard format for a type of value, for example dates in one location, and then you can use the pipe throughout the entire application so that the same format is used everywhere.
Pipes that are included with Angular:
- Date - Formats a date value according to locale rules
- UpperCase - Transforms text to all upper case
- LowerCase - Transforms text to all lower case
- Currency - Transforms a number to a formatted currency string based on locale rules
- Decimal - Transforms a number to a formatted string with a decimal point based on locale rules
- Percent - Transforms a number to a percentage string formatted according to locale rules
