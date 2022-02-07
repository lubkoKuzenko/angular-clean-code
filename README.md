# Angular Clean Code

<img src="https://webref.ru/assets/images/angular-cookbook/angular.png" width="960">

## Table of Content

- [Introduction](#Introduction)
- [Repo](#Repo)
- [Configuration](#Configuration)
  - [Configuring tsconfig.json](#Configuring-tsconfigjson)
  - [Configuring Angular ESLint](#Configuring-Angular-ESLint)
  - [Configuring HTMLHint](#Configuring-HTMLHint)
  - [Configuring stylelint](#Configuring-stylelint)
  - [Configuring Prettier](#Configuring-Prettier)
  - [Configuring Proxy for API Calls](#configuring-proxy-for-api-calls)
  - [Configuring Karma for CI/CD (bamboo example)](#karma-configuration-for-cicd-bamboo-example)
  - [Configuring Karma for CI/CD (azure example)](#karma-configuration-for-cicd-azure-example)
- [Architectural principles](#Architectural-principles)
  - [SOLID](#solid)
    - [Single responsibility](#single-responsibility)
    - [Open closed](#open-closed)
    - [Liskov substitution](#oiskov-substitution)
    - [Interface segregation](#interface-segregation)
    - [Dependency Inversion](#dependency-Inversion)
  - [Don't repeat yourself (DRY)](#code#dont-repeat-yourself-dry)
  - [Keep it short and simple (KISS)](#keep-it-short-and-simple-kiss-principle)
- [Angular Architecture](#angular-architecture)
  - [Project structure](#project-structure)
    - [AppModule](#AppModule)
    - [CoreModule](#CoreModule)
    - [SharedModule](#SharedModule)
    - [The Feature Modules](#The-Feature-Modules)
      - [Container Components](#Container-Components)
      - [Presentational Components](#Presentational-Components)
      - [Page Components](#Page-Components)
  - [Data flow architecture](#data-flow-architecture)
- [Mock API with miragejs](#mock-api-with-miragejs)
- [Change Detection](#Change-Detection)
- [State management](#state-management)
  - [Facade Design Pattern](#facade-design-pattern)
  - [@ngrx/component-store](#ngrxcomponent-store)
- [Angular Features](#Angular-Features)
  - [Attribute Directive](#Attribute-Directive)
  - [Structural Directives](#Structural-Directives)
  - [Pipe](#Pipe)
  - [NgTemplateOutlet](#NgTemplateOutlet)
  - [ngClass](#ngClass)
- [Angular Forms](#angular-forms)
  - [Basic setup](#Basic-setup)
  - [Nested Forms](#Nested-Forms)
  - [Dynamic Forms](#Dynamic-Forms)
  - [Custom FormGroup Validator](#Custom-FormGroup-Validator)
  - [Custom FormControl Validator](#Custom-FormControl-Validator)
  - [ControlValueAccessor](#ControlValueAccessor)
  - [Testing Forms](#Testing-Forms)
- [Angular Routing](#angular-routing)
  - [Componentless Route](#Componentless-Route)
  - [Route Resolvers](#Route-Resolvers)
  - [Custom RouteReuseStrategy](#Custom-RouteReuseStrategy)
- [Unit testing](#Unit-testing)
  - [How to test OnPush components](#how-to-test-onpush-components)
  - [How to test Custom Form Control Validator](#how-to-test-Custom-Form-Control-Validator)
  - [How to test Pipes](#how-to-test-Pipes)
  - [How to test Presentational components](#how-to-test-Presentational-components)
  - [How to test HTTP service](#how-to-test-HTTP-service)
- [Error Handling](#Error-Handling)
  - [Errors, Exceptions & CallStack](#errors-exceptions--callstack)
  - [Global Error Handler](#Global-Error-Handler)
- [JWT Token Interceptor](#JWT-Token-Interceptor)
- [Angular Dynamic Components](#Angular-Dynamic-Components)
- [Dynamic Importing 3rd-party Libraries](#dynamic-importing-3rd-party-libraries)
- [Unsubscribe from Observables](#Unsubscribe-from-Observables)
- [Containerizing Angular using Docker](#Containerizing-Angular-using-Docker)
- [Angular Schematics](#Angular-Schematics)
- [Performance](#Performance)
  - [Webpack Bundle Analyzer](#Webpack-Bundle-Analyzer)

## Introduction

In order to maintain high quality of delivery and prevent technical debt from being created, we had to agree to a series of guidelines and good practices of how to plan, structure and write applications in Angular

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Repo

Repo with Code: https://github.com/lubkoKuzenko/ng-start
<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Configuration

**Resources**

- ["TypeScript Deep Dive: tsconfig.json"](https://basarat.gitbook.io/typescript/project/compilation-context/tsconfig)

- ["tsconfig.json с комментариями"](https://gist.github.com/lubkoKuzenko/b0dfc526a8be2a00f007542960206260)

- ["Angular ESLint"](https://github.com/angular-eslint/angular-eslint#migrating-from-codelyzer-and-tslint/)

- ["Configuring HTMLHint"](https://github.com/htmlhint/HTMLHint/)

- ["List of HTMLHint rules"](https://github.com/htmlhint/HTMLHint/blob/master/docs/user-guide/list-rules.md/)

- ["Configuring stylelint"](https://github.com/stylelint/stylelint/)

- ["List of stylelint rules"](https://github.com/stylelint/stylelint/blob/master/docs/user-guide/rules/list.md/)

- ["Prettier Options"](https://prettier.io/docs/en/options.html/)

- ["Configure a proxy for your API calls with Angular CLI"](https://juristr.com/blog/2016/11/configure-proxy-api-angular-cli/)

- ["Setup a Proxy for API Calls for Your Angular CLI App"](https://medium.com/better-programming/setup-a-proxy-for-api-calls-for-your-angular-cli-app-6566c02a8c4d/)

### Configuring tsconfig.json

The presence of a tsconfig.json file in a directory indicates that the directory is the root of a TypeScript project. The tsconfig.json file specifies the root files and the compiler options required to compile the project.

```ts
// tsconfig.json
{
  "compileOnSave": false,
  "compilerOptions": {
    // Basic Options
    "target": "es5",                        // Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'.
    "module": "commonjs",                  // Specify module code generation: 'commonjs', 'amd', 'system', 'umd' or 'es2015'.
    "lib": [],                             // Specify library files to be included in the compilation:
    "allowJs": true,                       // Allow JavaScript files to be compiled.
    "checkJs": true,                       // Report errors in .js files.
    "jsx": "preserve",                     // Specify JSX code generation: 'preserve', 'react-native', or 'react'.
    "declaration": true,                   // Generates corresponding '.d.ts' file.
    "sourceMap": true,                     // Generates corresponding '.map' file.
    "outFile": "./",                       // Concatenate and emit output to single file.
    "outDir": "./",                        // Redirect output structure to the directory.
    "rootDir": "./",                       // Specify the root directory of input files. Use to control the output directory structure with --outDir.
    "removeComments": true,                // Do not emit comments to output.
    "noEmit": true,                        // Do not emit outputs.
    "importHelpers": true,                 // Import emit helpers from 'tslib'.
    "downlevelIteration": true,            // Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'.
    "isolatedModules": true,               // Transpile each file as a separate module (similar to 'ts.transpileModule').
​
    // Strict Type-Checking Options
    "strict": true,                        // Enable all strict type-checking options.
    "noImplicitAny": true,                 // Raise error on expressions and declarations with an implied 'any' type.
    "strictNullChecks": true,              // Enable strict null checks.
    "noImplicitThis": true,                // Raise error on 'this' expressions with an implied 'any' type.
    "alwaysStrict": true,                  // Parse in strict mode and emit "use strict" for each source file.
​
    // Additional Checks
    "noUnusedLocals": true,                // Report errors on unused locals.
    "noUnusedParameters": true,            // Report errors on unused parameters.
    "noImplicitReturns": true,             // Report error when not all code paths in function return a value.
    "noFallthroughCasesInSwitch": true,    // Report errors for fallthrough cases in switch statement.
​
    // Module Resolution Options
    "moduleResolution": "node",            // Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6).
    "baseUrl": "./",                       // Base directory to resolve non-absolute module names.
    "paths": {},                           // A series of entries which re-map imports to lookup locations relative to the 'baseUrl'.
    "rootDirs": [],                        // List of root folders whose combined content represents the structure of the project at runtime.
    "typeRoots": [],                       // List of folders to include type definitions from.
    "types": [],                           // Type declaration files to be included in compilation.
    "allowSyntheticDefaultImports": true,  // Allow default imports from modules with no default export. This does not affect code emit, just typechecking.
​
    // Source Map Options
    "sourceRoot": "./",                    // Specify the location where debugger should locate TypeScript files instead of source locations.
    "mapRoot": "./",                       // Specify the location where debugger should locate map files instead of generated locations.
    "inlineSourceMap": true,               // Emit a single file with source maps instead of having a separate file.
    "inlineSources": true,                 // Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set.
​
    // Experimental Options
    "experimentalDecorators": true,        // Enables experimental support for ES7 decorators.
    "emitDecoratorMetadata": true          // Enables experimental support for emitting type metadata for decorators.
  }
}
```

### Configuring Angular ESLint

Create file `.eslintrc` in root folder
```ts
{
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "extends": ["prettier", "prettier/@typescript-eslint"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "tsconfig.json",
    "sourceType": "module"
  },
  "plugins": [
    "eslint-plugin-import",
    "@angular-eslint/eslint-plugin",
    "@typescript-eslint",
    "@typescript-eslint/tslint"
  ],
  "rules": {
    "@angular-eslint/directive-selector": ["error", { "type": "attribute", "prefix": "bb", "style": "camelCase" }],
    "@angular-eslint/component-selector": ["error", { "type": "element", "prefix": "bb", "style": "kebab-case" }],
    "@angular-eslint/component-class-suffix": "error",
    "@angular-eslint/directive-class-suffix": "error",
    "@angular-eslint/no-input-rename": "error",
    "@angular-eslint/no-output-on-prefix": "error",
    "@angular-eslint/no-output-rename": "error",
    "@angular-eslint/use-pipe-transform-interface": "error",
    "@typescript-eslint/consistent-type-definitions": "error",
    "@typescript-eslint/dot-notation": "off",
    "@typescript-eslint/explicit-member-accessibility": [
      "off",
      {
        "accessibility": "explicit"
      }
    ],
    "@typescript-eslint/member-ordering": "error",
    "@typescript-eslint/naming-convention": "error",
    "@typescript-eslint/no-empty-function": "off",
    "@typescript-eslint/no-empty-interface": "error",
    "@typescript-eslint/no-inferrable-types": [
      "error",
      {
        "ignoreParameters": true
      }
    ],
    "@typescript-eslint/no-misused-new": "error",
    "@typescript-eslint/no-non-null-assertion": "error",
    "@typescript-eslint/no-unused-expressions": "error",
    "@typescript-eslint/prefer-function-type": "error",
    "@typescript-eslint/quotes": ["error", "double"],
    "@typescript-eslint/tslint/config": [
      "error",
      {
        "rules": {
          "whitespace": true
        }
      }
    ],
    "@typescript-eslint/unified-signatures": "error",
    "arrow-body-style": "error",
    "constructor-super": "error",
    "eqeqeq": ["error", "smart"],
    "guard-for-in": "error",
    "id-blacklist": "off",
    "id-match": "off",
    "import/no-deprecated": "warn",
    "no-bitwise": "error",
    "no-caller": "error",
    "no-console": [
      "error",
      {
        "allow": [
          "log",
          "dirxml",
          "warn",
          "error",
          "dir",
          "timeLog",
          "assert",
          "clear",
          "count",
          "countReset",
          "group",
          "groupCollapsed",
          "groupEnd",
          "table",
          "Console",
          "markTimeline",
          "profile",
          "profileEnd",
          "timeline",
          "timelineEnd",
          "timeStamp",
          "context"
        ]
      }
    ],
    "no-debugger": "error",
    "no-empty": "off",
    "no-eval": "error",
    "no-fallthrough": "error",
    "no-new-wrappers": "error",
    "no-restricted-imports": [
      "error",
      {
        "paths": ["rxjs/Rx"],
        "patterns": ["rxjs/(?!operators|testing)"]
      }
    ],
    "no-shadow": [
      "error",
      {
        "hoist": "all"
      }
    ],
    "no-throw-literal": "error",
    "no-undef-init": "error",
    "no-underscore-dangle": "off",
    "no-unused-labels": "error",
    "no-var": "error",
    "prefer-const": "error",
    "radix": "error",
    "spaced-comment": [
      "error",
      "always",
      {
        "markers": ["/"]
      }
    ],
    "indent": "off"
  }
}
```

Add command to script section of `package.json`

```ts
 "lint:ts": "eslint --color -c .eslintrc --ext .ts .",
```

### Configuring HTMLHint

1) Install htmlhint

```npm
npm install --save-dev htmlhint
```

2) Create a .htmlhintrc configuration file in the root of your project:

```.htmlhintrc
{
  "attr-value-not-empty": false
}
```

3) Run HTMLHint on, for example, all the HTML files in your project:

```npm 
"lint:html": "npx htmlhint \"src\" --config .htmlhintrc",
```
4) Configure rules based on rules list: https://github.com/htmlhint/HTMLHint/blob/master/docs/user-guide/list-rules.md

### Configuring stylelint

1) Install stylelint

```npm
npm install --save-dev stylelint stylelint-config-standard
```

2) Create a .htmlhintrc configuration file in the root of your project:

```.stylelintrc
{
  "extends": "stylelint-config-standard"
}
```

3) Run HTMLHint on, for example, all the HTML files in your project:

```npm 
 "lint:scss": "npx stylelint \"src/**/*.scss\" --syntax scss",
```
4) Configure rules based on rules list: https://github.com/stylelint/stylelint/blob/master/docs/user-guide/rules/list.md


### Configuring Prettier

```ts
// .prettierrc
printWidth: 120
tabWidth: 2
semi: true
singleQuote: false
trailingComma: all # other options `es5` or `all`
bracketSpacing: true
arrowParens: always # other option "always"
htmlWhitespaceSensitivity: 'ignore'
```

### Configuring Proxy for API Calls

When we develop an Angular app which needs a back end to persist data, the back end is often served on another port of localhost. For example, the URL to the front end Angular app is `http://localhost:4200`, while the URL to the back end server is `http://localhost:3000`. In this case, if we make an HTTP request from the front end app to the back end server, it is a cross-domain request and we need to do some extra work to make it happen

Angular CLI uses `webpack-dev-server` as the development server. The `webpack-dev-server` makes use of the powerful `http-proxy-middleware` package which allows us to send API requests on the same domain when we have a separate API back end development server

<img src="./assets/ngdevserver-proxy.png" width="100%" />

1) Create a file called `proxy.conf.json` next to our project’s `package.json`

2) Add the following contents to the newly created `proxy.conf.json` file:

  ```json
  {
    "/folder/sub-folder/*": {
      "target": "http://localhost:1100",
      "secure": false,
      "pathRewrite": {
        "^/folder/sub-folder/": "/new-folder/"
      },
      "changeOrigin": true,
      "logLevel": "debug"
    }
  }
  ```

3) Edit the `package.json` file’s start script to be:
  ```npm 
  "start": "ng serve --proxy-config proxy.conf.json",
  ```
4) Relaunch the `npm start` process to make our changes effective

#### Options: 

`/folder/sub-folder/*` - path says: When I see this path inside my angular app I want to do something with it. The * character indicates that everything that follows the sub-folder will be included.

`target` - "http://localhost:1100" for the path above make target URL the host/source, therefore in the background we will have.

`pathRewrite` - { "^/folder/sub-folder/": "/new-folder/" }, Now let's say that you want to test your app locally, the url http://localhost:1100/folder/sub-folder/ may contain an invalid path: /folder/sub-folder/. You want to change that path to a correct one which is http://localhost:1100/new-folder/, therefore the pathRewrite will become useful. It will exclude the path in the app(left side) and include the newly written one (right side)

`secure` - represents wether we are using http or https. If https is used in the target attribute then set secure attribute to true otherwise set it to false

`changeOrigin` - option is only necessary if your host target is not the current environment, for example: localhost. If you want to change the host to www.something.com which would be the target in the proxy then set the changeOrigin attribute to "true":

`logLevel` - attribute specifies wether the developer wants to display proxying on his terminal/cmd, hence he would use the "debug" value as shown in the image

### Karma configuration for CI/CD (bamboo example)

```js
module.exports = function(config) {
  config.set({
    basePath: "",
    frameworks: ["jasmine", "@angular-devkit/build-angular"],
    plugins: [
      require("karma-jasmine"),
      require("karma-chrome-launcher"),
      require("karma-bamboo-reporter"),
      require("karma-jasmine-html-reporter"),
      require("karma-coverage-istanbul-reporter"),
      require("@angular-devkit/build-angular/plugins/karma")
    ],
    client: {
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      dir: require("path").join(__dirname, "../../coverage/"),
      reports: ["html", "lcovonly", "clover"],
      fixWebpackSourcePaths: true,
      "report-config": {
        html: {
          subdir: "html"
        },
        clover: {
          subdir: "clover"
        }
      }
    },
    bambooReporter: {
      filename: "coverage/mocha.json"
    },
    reporters: ["progress", "kjhtml", "bamboo"],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    // browsers: ["Chrome"], // enable this line locally for testing and debugging
    browsers: ["ChromeHeadlessNoSandbox"],
    customLaunchers: {
      ChromeHeadlessNoSandbox: {
        base: "ChromeHeadless",
        flags: ["--no-sandbox"],
        options: {
          viewportSize: {
            width: 1280,
            height: 1024
          }
        }
      }
    },
    singleRun: false,
    restartOnFileChange: true
  });
};
```

### Configuring Karma for CI/CD (azure example)
```ts
module.exports = function(config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-trx-reporter'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter'),
      require('@angular-devkit/build-angular/plugins/karma'),
      require('karma-spec-reporter')
    ],
    client: {
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      dir: 'test-results/coverage',
      reports: ['html', 'lcovonly', 'text-summary', 'cobertura'],
      fixWebpackSourcePaths: true
    },
    reporters: ['progress', 'kjhtml', 'trx', 'spec', 'coverage-istanbul'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    customLaunchers: {
      ChromeDebugging: {
        base: 'Chrome',
        flags: ['--remote-debugging-port=9222']
      },
      ChromeHeadlessNoSandbox: {
        base: 'Chrome',
        flags: [
          '--no-sandbox',
          '--disable-setuid-sandbox',
          '--headless',
          '--disable-gpu',
          '--remote-debugging-port=9222'
        ]
      }
    },
    singleRun: false,
    trxReporter: {
      outputFile: 'test-results/test-results.trx',
      shortTestName: false
    }
  });
};
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Architectural principles

### SOLID

In object-oriented computer programming, `SOLID` is an acronym for five design principles intended to make software designs more understandable, flexible and maintainable.

`S` — Single responsibility principle

`O` — Open closed principle

`L` — Liskov substitution principle

`I` — Interface segregation principle

`D` — Dependency Inversion principle

#### Single responsibility

`A class should have one and only one reason to change, meaning that a class should only have one job.`

Following this principle helps to produce more loosely coupled and modular systems, since many kinds of new behavior can be implemented as new classes, rather than by adding additional responsibility to existing classes. Adding new classes is always safer than changing existing classes, since no code yet depends on the new classes.

When this principle is applied to application architecture and taken to its logical endpoint, you get microservices. A given microservice should have a single responsibility. If you need to extend the behavior of a system, it's usually better to do it by adding additional microservices, rather than by adding responsibility to an existing one.

#### Open closed

`Objects or entities should be open for extension, but closed for modification.`

`Open for extension` means that we should be able to add new features or components to the application without breaking existing code.

`Closed for modification` means that we should not introduce breaking changes to existing functionality, because that would force you to refactor a lot of existing code

#### Liskov substitution

`Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.`

Subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.

#### Interface segregation

`A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use`

#### Dependency Inversion

`Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions`

### Don't repeat yourself (DRY)

The application should avoid specifying behavior related to a particular concept in multiple places as this practice is a frequent source of errors. At some point, a change in requirements will require changing this behavior. It's likely that at least one instance of the behavior will fail to be updated, and the system will behave inconsistently.

Rather than duplicating logic, encapsulate it in a programming construct. Make this construct the single authority over this behavior, and have any other part of the application that requires this behavior use the new construct.

### Keep it Short and Simple (KISS Principle)

Keep it simple, stupid (KISS) is a design principle which states that designs and/or systems should be as simple as possible. Wherever possible, complexity should be avoided in a system—as simplicity guarantees the greatest levels of user acceptance and interaction.

## Angular Architecture

**Resources**

- ["Angular Application Architecture"](https://bulldogjob.pl/articles/539-scalable-angular-application-architecture)
- ["Angular File Structure and Best Practices"](https://medium.com/@shijin_nath/angular-right-file-structure-and-best-practices-that-help-to-scale-2020-52ce8d967df5)
- ["How to define a highly scalable folder structure for your Angular project"](https://itnext.io/choosing-a-highly-scalable-folder-structure-in-angular-d987de65ec7)
- ["Component Communication in Angular"](https://www.digitalocean.com/community/tutorials/angular-component-communication)
- ["Designing Scalable Angular Apps: Pages, Containers and Views"](https://blog.bitsrc.io/designing-scalable-angular-apps-pages-containers-and-views-ac9cd83afa2d)

### What is a scalable architecture?

First of all, what does it mean that GUI application is scalable? GUI always runs as a single, separate application for every user, so there is no "high number of users" challenge that is typical for backend applications. Instead, GUI has to deal with the following scalability factors: increasing size of data loaded to the application, growing complexity and size of the project, usually followed by longer loading times.

There is also a part of the problem that is not visible from the outside, namely how an application scales from the programmer’s point of view. An application with bad, or not scalable architecture, tends to be very hard to develop after some time. Increasing complexity, technical debt and simply code smell, has a direct impact on project estimates, costs and the quality of the overall solution.

Good architecture does not guarantee that above problems will not occur in your application, however, it gives the development team a powerful tool to reduce and even eliminate most of the issues that they might encounter.

In short, well designed architecture should work equally good for small and big applications, should provide very good user experience regardless of the application’s size and amount of processed data. Additionally, it should provide a set of clear and easy to follow rules for developers in order to sustain the quality of the project. And finally, it should be simple and preferably based on widely accepted design patterns. We want to keep the learning curve of our applications as small as possible.

### Project structure

Application modules are clearly visible in the file tree, as separate directories. Every module directory contains all files (code, styles, templates etc.) that are related to a given module. A very important element of this approach is isolation of modules. Simply speaking, it means that every module is self-contained and does not refer to files from different modules, so, theoretically, you can delete one of them from the application, and the rest will work without any problems.

<img src="./assets/content_1.png" width="373" height="549">

Obviously, it’s not possible to strictly follow this rule in the real world. At least some services and components have to be reused across the whole application. Therefore, some parts of application functionality are stored in "Core" and "Shared" modules. Now our application structure looks like this:

<img src="./assets/imports.png" width="720" height="420" style="background-color: #ffffff; padding: 2rem">

As you can see, there are now three main modules in the project:

#### AppModule

In Angular, everything is organized in modules, and every application have at least one of them, the app root module. The app module is the entry point of the application, and is the module that Angular uses to bootstrap the application. The setup instructions when creating a new application produces a minimal `AppModule` with a single component. You’ll evolve this module as the application grows.

```ts
import { NgModule } from "@angular/core";

import { CoreModule } from "./core";
import { SharedModule } from "./shared";

import { AppRoutingModule } from "./app-routing.module";
import { AppComponent } from "./app.component";

import { DashboardModule } from "./dashboard/dashboard.module";

@NgModule({
  declarations: [AppComponent],
  imports: [
    CoreModule,
    SharedModule,

    // features
    DashboardModule,

    // app
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

#### CoreModule

The `CoreModule` takes on the role of the app root module, but is not the module that gets bootstrapped by Angular at run-time. The common denominator between the files present here is that we only need to load them once, and that is at run-time, which makes them singleton. The module contains root-scoped services, static components like the navbar and footer, interceptors, guard, constants, enums, utils, and universal models. To prevent re-importing the module elsewhere, we should add a module-import-guard in it’s constructor method.

`Core Module` is only going to be imported into our root module, so this module, normally, don’t have really any exports.

Structure of Core module:

```bash
├── app
|  ├── core
|  |  ├── guards
|  |  |   └── module-import-guard.ts
|  |  ├── interceptor
|  |  |   ├── error-interceptor.ts
|  |  |   └── jwt-interceptor.ts
|  |  ├── services
|  |  |   ├── app-init.service.ts
|  |  |   └── router-reuse.strategy.ts
|  |  └── core.module.ts
|  ├── app-routing.module.ts
|  └── app.module.ts
├── favicon.ico
├── index.html
├── main.ts
├── styles.scss
└── test.ts
```

```ts
import { NgModule, Optional, SkipSelf, ErrorHandler } from "@angular/core";
import { CommonModule } from "@angular/common";
import { HttpClientModule, HTTP_INTERCEPTORS } from "@angular/common/http";
import { TokenInterceptor } from "./interceptor/jwt-interceptor";
import { AppErrorInterceptor } from "./interceptor/error-interceptor";

import { AppInitService } from "./services/app-init.service";
export function initializerFactory(appConfig: AppInitService) {
  return (): Promise<any> => {
    return appConfig.load();
  };
}

@NgModule({
  imports: [CommonModule, HttpClientModule],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: TokenInterceptor,
      multi: true
    },
    {
      provide: APP_INITIALIZER,
      useFactory: initializerFactory,
      deps: [AppInitService],
      multi: true
    },
    {
      provide: ErrorHandler,
      useClass: AppErrorInterceptor
    }
  ],
  exports: [HttpClientModule]
})
export class CoreModule {
  constructor(
    @Optional()
    @SkipSelf()
    parentModule: CoreModule
  ) {
    if (parentModule) {
      throw new Error("CoreModule is already loaded. Import only in AppModule");
    }
  }
}
```

#### SharedModule

`Shared Module` is the one that almost always will have some exports, because otherwise it wouldn’t be shareable, for sure.

Usually a set of components or services that will be reused in other application modules, not applied globally. They can be imported by feature modules.

Structure of `Shared` module:

```bash
├── app
|  ├── shared
|  |  ├── modules
|  |  |  ├── primeng.module.ts
|  |  |  ├── material.module.ts.
|  |  |  └── *.module.ts
|  |  ├── components
|  |  |  ├── *.component.ts
|  |  |  └── components.module.ts
|  |  ├── directives
|  |  |  ├── *.directive.ts
|  |  |  └── directives.module.ts
|  |  └── pipes
|  |    ├── *.pipe.ts
|  |    └── pipes.module.ts
|  └── shared.module.ts
├── index.html
├── main.ts
├── styles.scss
└── test.ts
```

```ts
// shared.module.ts
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { FormsModule, ReactiveFormsModule } from "@angular/forms";
import { BrowserModule } from "@angular/platform-browser";
import { BrowserAnimationsModule } from "@angular/platform-browser/animations";

import { SharedDirectivesModule } from "./directives/directives.module";
import { SharedPipesModule } from "./pipes/pipes.module";
import { SharedComponentsModule } from "./components/components.module";

const SHARED_MODULES = [
  CommonModule,
  FormsModule,
  ReactiveFormsModule,

  SharedDirectivesModule,
  SharedPipesModule,
  SharedComponentsModule
];

@NgModule({
  providers: [],
  declarations: [],
  imports: [...SHARED_MODULES],
  exports: [...SHARED_MODULES]
})
export class SharedModule {}
```

directives.module.ts, pipes.module.ts, components.module.ts has the same structure:

```ts
// components.module.ts
import { CommonModule } from "@angular/common";
import { NgModule, Type } from "@angular/core";
import { ReactiveFormsModule } from "@angular/forms";

export const SHARED_COMPONENTS: Array<Type<any>> = [];

@NgModule({
  imports: [CommonModule, ReactiveFormsModule],
  declarations: [...SHARED_COMPONENTS],
  exports: [...SHARED_COMPONENTS]
})
export class SharedComponentsModule {}
```

All remaining modules (so-called feature modules) should be isolated and independent. Such a structure not only allows for clear concerns separation, but is also a convenient starting point for implementing lazy loading functionality, another crucial step in preparing a scalable application architecture.

<img src="./assets/1_22skrCDLM6C3oRTuIecIng.png" width="100%" />

#### The Feature Modules

`Feature Module` is normally a standalone and it will be imported just into the root.

The initial Angular application does only have one single module, which works great for small applications. But as the application grows, you’ll need to consider subdividing it into multiple feature modules, some which can be lazy loaded. These modules should only depend on the SharedModule, and their functionality should be scoped to the module.

`Feature` modules deliver user experience dedicated to a particular application feature like the user- or the administration-part of the app. We’re grouping the components, services, models and other functionality that belongs together. They typically have a top component that acts as the feature root and private, supporting sub-components descend from it. They might be imported by the root AppModule of a small application that lacks routing or need to show some initial content, but can also be lazy loaded with references in the app routing file.

Domain feature modules rarely have providers, but when they do, the lifetime of the provided services should be the same as the lifetime of the module. Beginning with Angular 6.0, the preferred way pf creating a singleton is to set providedIn to root on the service’ @Injectable decorator. This tells Angular to provide the service in the application root. But we can also use this to create a singleton service is to set providedIn to root on the service's @Injectable() decorator. This tells Angular to provide the service in the application root. We can use this in the context of a feature by using the providedIn property on the module instead, resulting in an error when using it elsewhere.

When you need the service in other modules as well, it probably belongs in the CoreModule’ service’s declaration instead.

Structure of `Feature` module:

```bash
├── app
|  ├── @core
|  ├── @shared
|  ├── feature_one
|  |   ├── components
|  |   |   ├── component 1
|  |   |   ├── component 2
|  |   |   ├── component 3
|  |   |   └── components.module.ts
|  |   ├── containers
|  |   |   ├── container 1
|  |   |   ├── container 2
|  |   |   ├── container 3
|  |   |   └── containers.module.ts
|  |   ├── page
|  |   |   ├── page
|  |   |   └── page.module.ts
|  |   ├── feature_one-routing.module.ts
|  |   └── feature_one.module.ts
|  ├── feature_two
|  └── feature_three
├── index.html
├── main.ts
├── styles.scss
└── test.ts
```

To give an overview, the Page Component has Container components as children. Then each Container component has Presentational components as children (Pages -> Containers -> Views).

#### Presentational Components

Presentational components are UI components that comprise the visual elements. They are dumb components that accept data from outside and triggers events for actions like button click. These custom components are typically a composition of UI elements, from libraries like `ng-bootstrap` or `Angular Material` created for business functionality.

We can unit test these View components easily to test the actions and visualization of data since they don’t have any direct dependencies with services or external states.

- are purely user interface and concerned with how things look.
- are not aware about the business logic, or services.
- receive data via @Inputs, and emit events via @Output.

#### Container Components

Container components are the components that bind, Presentational components, services, and state management together to deliver business functionality.

The Container components use the services to fetch data from backend and bind them to the Presentational components. They also listen to the events coming from Presentational components and update the state of each Presentational components and communicates with the backend.

Container components are the hub of connecting things, dealing with business logic, delegating the presentation to View components.

- contain all the business logic.
- pass the data to the Presentational Components, and handle @Output events raised by them.
- have no UI logic.
- do have dependencies on other parts of your app, like services, or your state store.

#### Page Components

Page components are the components that define the layout of a page based on URL paths. For instance, you can compose a page component with a header, footer, and an area in the middle. So basically, you can define a Page component for each parent route. Inside each Page component, you can place your Container components. In some instances, you can place Presentational components directly inside the Page components, when the view components don’t need any dynamic behavior.

By using Page components, it will help the Container components to know less bout the fixed layout of a page and focus more on dynamic component placement based on business logic. It will also help to reduce the depth of Container components to reuse the layout across different routes.

### Data flow architecture

In modern SPA frameworks, everything is a component. They are the main building blocks for creating and controlling user interfaces. Angular (and other modern frameworks) will organize components in a hierarchical tree, which means that components can have a parent and children. Let’s imagine that our components communicate with each other with no rules. Any one of them would be sending data and firing events to each other and after a while, it would become very messy and we would be lost in the woods of data requests and responses.

With this kind of organization, we need to assure unidirectional data flow within our parent and child components. The main rule is that actions go up and data flows down. Every component will accept @Input() parameters to receive the data from their parent and be able to send the @Output() event to notify subscribers that something has happened.

<img src="./assets/angular-project-architecture-diagram2.png" width="100%" style="background-color: #ffffff; padding: 2rem">

In our application we've introduced the idea of "smart" and "dummy" components. The smart components are also called "Containers". The idea behind this division is to clearly define the parts of the application that contain some logic, communicate with services and cause side effects (like service calls, state updates etc.). Every such action is implemented only in Containers. On the contrary, "stupid" components have very little or no logic at all. All the data they need is passed by @Input parameters. If a component wants to communicate with the outside word, it has to emit an event (via @Output attribute).

<img src="./assets/content_6.png" width="641" height="201" style="background-color: #ffffff; padding: 2rem">

Such architectural approach is intended to keep the number of Containers as small as possible. The more components in the application are "dummy", the simpler is the data flow and the easier it is to work with it. Deciding which component should take the role of a Container and which should be just a plain component is not a trivial task and needs to be resolved per particular case. However, usually the first step we take is assuming that a main screen component should be the smart one, as in the example below, when the container is marked with blue color and simple components are gray.

Such an approach to architecture is not only about readability of code and organized data flow. Dummy component are much easier to test. Their state is entirely induced by the Input they are provided with, they cause no side effect and the result of any component action is visible as a proper event being fired.

What is more, such behavior nicely corresponds with performance optimization of Angular’s change detection process. The change detection strategy for dummy components can be set to "onPush" which will trigger the change detection process for the component only when the input properties have been modified. It's an easy and very efficient method of optimizing Angular applications.

## Mock API with mirage.js

**Resources**

- ["Mirage.js"](https://miragejs.com/)

### Installation

To add Mirage to your project, run

```npm
npm install --save-dev miragejs
```

### Usage

Create folder where all mocks will stay. In my case it's `_be-mocks/`. Then inside it we need `index.ts` file with Server definition.

```ts
// index.ts
import { Server } from "miragejs";
import * as users from "./users.json";

export default () => {
  new Server({
    seeds(server) {
      server.db.loadData({
        users: (users as any).default,
      });
    },
    routes() {
      this.namespace = "/api";

      this.get("/users", (schema) => schema.db.users[0]);
    },
  });
};

```

After server configurations we need to add it to `app.module.ts`

```ts
// app.module.ts
import mockServer from "./_be-mocks";

mockServer();
```

On service we are able to get data in this way:

```ts
getUsers() {
  return this.http.get("/api/users");
}
```

To Allows importing modules with a ‘.json’ extension add to your tsconfig.json

```ts
"resolveJsonModule": true
```


## Change Detection

**Resources**

- ["Change Detection in Angular"](https://vsavkin.com/change-detection-in-angular-2-4f216b855d4c)
- ["Everything you need to know about change detection in Angular"](https://blog.angularindepth.com/everything-you-need-to-know-about-change-detection-in-angular-8006c51d206f)
- ["The Last Guide For Angular Change Detection You'll Ever Need"](https://www.mokkapps.de/blog/the-last-guide-for-angular-change-detection-you-will-ever-need/)

This mechanism of syncing the HTML with our data is called `Change Detection`

#### How Change Detection Works

- Developer updates the data model
- Angular detects the change
- Change detection checks every component in the component tree from top to bottom to see if the corresponding model has changed
- If there is a new value, it will update the component’s view (DOM)

By default, Angular `Change Detection` checks for all components from top to bottom if a template value has changed

#### Change Detection Strategies

Angular provides two strategies to run change detections:
- Default
  By default, Angular uses the `ChangeDetectionStrategy.Default` change detection strategy. This default strategy checks every component in the component tree from top to bottom every time an event triggers change detection (like user event, timer, XHR, promise and so on). This conservative way of checking without making any assumption on the component’s dependencies is called dirty checking. It can negatively influence your application’s performance in large applications which consists of many components

  <img src="./assets/cd-cycle.gif" />

- OnPush
  We can switch to the `ChangeDetectionStrategy.OnPush` change detection strategy by adding the changeDetection property to the component decorator metadata

  <img src="./assets/0_LHWjj-bxWhxjMYiG.gif" />

  The `OnPush` change detection strategy allows us to disable the change detection mechanism for subtrees of the component tree. By setting the change detection strategy to any component to the value `ChangeDetectionStrategy.OnPush` will make the change detection perform **only** when the component has received different inputs. Angular will consider inputs as different when it compares them with the previous inputs by reference, and the result of the reference check is `false`. In combination with [immutable data structures](https://facebook.github.io/immutable-js/), `OnPush` can bring great performance implications for such "pure" components.

## State management


**Resources**

- ["An Angular Architect's First Experiences"](https://www.thinktecture.com/en/angular/should-i-use-ngrx-an-agular-achitects-experiences/)

### Facade Design Pattern

Facade discusses encapsulating a complex subsystem within a single interface object. This reduces the learning curve necessary to successfully leverage the subsystem. It also promotes decoupling the subsystem from its potentially many clients.
The Facade object should be a fairly simple advocate or facilitator. It should not become an all-knowing oracle or “god” object.
Here is the good read for Facade design pattern in details

<img src="./assets/example-of-facade-designpattern-in-uml.png" width="100%">

I would recommend following steps to build Angular services using Facade pattern:

- Define all your Angular services as per your business requirement and/or keep adding more as needed.
- Create a service called “FacadeService” (feel free to use any other name here)
- Create a Module and provide all Angular services

```ts
// user.service.ts
import { Injectable } from "@angular/core";

@Injectable()
export class UserService {
  constructor() {}

  getUsers() {
    return [{ name: "test" }];
  }
}
```

```ts
// facade.service.ts
@Injectable()
export class FacadeService {
  // users
  public users$ = new BehaviorSubject<User[]>([]);

  constructor(public userService: UserService) {}

  public getUsers() {
    return this.users$.next(this.userService.getUsers());
  }
}
```

```ts
// ./containers/users.component.ts
@Component({
  templateUrl: "./users.html",
  styleUrls: ["./users.scss"]
})
export class UsersComponent implements OnInit {
  public users$ = this.facadeService.users$;

  constructor(public facadeService: FacadeService) {}

  public ngOnInit(): void {
    this.facadeService.getUsers();
  }
}
```

```html
<!-- ./containers/users.component.html  -->
{{ users$ | async | json }}
```

```ts
// feature.module.ts
@NgModule({
  imports: [CommonModule],
  declarations: [],
  providers: [UserService, FacadeService]
})
export class FeatureModule {}
```

### @ngrx/component-store

The component store is a local, stand-alone, store-like implementation, similar to the "Subject in a Service" pattern, offering a standardized store-like API for you.

#### It has only three very simple concepts that you have to learn:

`Selectors`: You select and subscribe to the state, either all or parts of it.

`Updater`: To update the state. It can be parts or in whole.

`Effects`: It is also to update the state but do some other necessary task beforehand. For example, an HTTP request to an API.

#### Adding the @ngrx/component-store

1) `npm install @ngrx/component-store --save`
2) create `*.store.ts` file in `store` folder
3) provide store service in component where it will be used `providers: [*Store]`
4) add dependency to constructor `constructor(private readonly store: *Store) {}`
5) use available methods `this.store`

#### CRUD Example - https://github.com/lubkoKuzenko/ng-start/tree/master/src/app/unit-cards

```typescript
import { Injectable } from "@angular/core";
import { ComponentStore, tapResponse } from "@ngrx/component-store";
import { Observable } from "rxjs";
import { switchMap } from "rxjs/operators";
import { Card } from "../interfaces";
import { CardsService } from "../services/cards.service";

// The state model
export interface CardsState {
  cards: Card[];
  loading: boolean;
}

const defaultState: CardsState = {
  cards: [],
  loading: false,
};

@Injectable()
export class CardsStore extends ComponentStore<CardsState> {
  constructor(public cardsService: CardsService) {
    super(defaultState);
  }

  // SELECTORS
  readonly cards$: Observable<Card[]> = this.select((state) => state.cards);
  readonly loading$: Observable<boolean> = this.select((state) => state.loading);

  // EFFECTS
  public readonly loadCards = this.effect((trigger$: Observable<void>) =>
    trigger$.pipe(
      switchMap(() => {
        this.patchState({ loading: true });

        return this.cardsService.getCards().pipe(
          tapResponse(
            (cards) => this.patchState((state) => ({ ...state, cards: cards || [], loading: false })),
            (_) => this.patchState({ cards: [] }),
          ),
        );
      }),
    ),
  );

  public readonly addCard = this.effect((trigger$: Observable<Card>) =>
    trigger$.pipe(
      switchMap((card: Card) => {
        this.patchState({ loading: true });

        return this.cardsService.addCard(card).pipe(
          tapResponse(
            (newCard: Card) =>
              this.patchState((state) => ({
                ...state,
                cards: [...state.cards, newCard],
                loading: false,
              })),
            (_) => this.patchState((state) => ({ cards: state.cards })),
          ),
        );
      }),
    ),
  );

  public readonly updateCard = this.effect((trigger$: Observable<Card>) =>
    trigger$.pipe(
      switchMap((card: Card) => {
        this.patchState({ loading: true });

        return this.cardsService.updateCard(card).pipe(
          tapResponse(
            () =>
              this.patchState((state) => {
                const updatedCards = state.cards.map((c: Card) => (c.id === card.id ? { ...c, ...card } : c));

                return {
                  ...state,
                  cards: [...updatedCards],
                  loading: false,
                };
              }),
            (_) => this.patchState((state) => ({ cards: state.cards })),
          ),
        );
      }),
    ),
  );

  public readonly removeCard = this.effect((trigger$: Observable<string>) =>
    trigger$.pipe(
      switchMap((cardId: string) => {
        this.patchState({ loading: true });

        return this.cardsService.deleteCard(cardId).pipe(
          tapResponse(
            () =>
              this.patchState((state) => {
                const updatedCards = state.cards.filter((card) => card.id !== cardId);

                return {
                  ...state,
                  cards: [...updatedCards],
                  loading: false,
                };
              }),
            (_) => this.patchState((state) => ({ cards: state.cards })),
          ),
        );
      }),
    ),
  );
}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Features

**Resources**

- ["How to Use ngTemplateOutlet"](https://www.tektutorialshub.com/angular/ngtemplateoutlet-in-angular/)

- ["ngTemplateOutlet: The secret to customisation"](https://indepth.dev/posts/1405/ngtemplateoutlet/)

- ["Angular NgClass Example – How to Add Conditional CSS Classes"](https://www.freecodecamp.org/news/angular-ngclass-example/)

- ["Writing your own structural directives with context variables"](https://github.com/JanMalch/ngx-code-dump/tree/master/custom%20directives/)

### Directives

<img src="./assets/1_OhepgWassGUMkQ6v8VtK3g.png" width="100%" />

### Attribute Directive
Directive should be stored in Directives folder of Shared Module.

```ts
// underline.directive.ts
import {
  Directive,
  ElementRef,
  HostListener,
  HostBinding,
  Renderer2
} from "@angular/core";

// Annotation section
@Directive({ selector: "[bbUnderline]" })

/*
 *element-name: select by element name.
 *.class: select by class name.
 *[attribute]: select by attribute name.
 *[attribute=value]: select by attribute name and value.
 *:not(sub_selector): select only if the element does not match the sub_selector.
 *selector1, selector2: select if either selector1 or selector2 matches.
 */
export class UnderlineDirective {
  // @Input() my: boolean;
  constructor(private el: ElementRef, private renderer: Renderer2) {}

  // HostBinding - will bind property to host element, If a binding changes, HostBinding will update the host element.
  // @HostBinding('style.backgroundColor')
  // color = 'yellow';

  // HostListener - will listen to the event emitted by host element, declared with @HostListener.
  @HostListener("mouseenter")
  onMouseEnter() {
    this.hover(true);
  }

  @HostListener("mouseleave")
  onMouseLeave() {
    this.hover(false);
  }

  hover(shouldUnderline: boolean) {
    if (shouldUnderline) {
      this.renderer.setStyle(
        this.el.nativeElement,
        "text-decoration",
        "underline"
      );
    } else {
      this.renderer.setStyle(this.el.nativeElement, "text-decoration", "none");
    }
  }
}
```

### Structural Directives
`Structural Directive` in Angular are responsible for manipulating, modifying and removing elements inside a template of a component. A structural directive is applied on a main element and according to the behavior of structural directive, it modifies and updates the main elements and its child elements. We have some inbuilt structural directives in Angular like `ngFor`, `ngSwitch` and `ngIf`

### Creating a custom structural directive

```ts
@Directive({
  selector: '[delayRendering]'
})
export class DelayRenderingDirective {
  constructor(
    private template: TemplateRef<any>,
    private container: ViewContainerRef
  ) { }

  @Input()
  set delayRendering(delayTime: number): void { }

  // Rendering
  ngOnInit() {
    this.createView();
  }

  private createView() {
    this.container.clear();
    this.container.createEmbeddedView(this.template);
  }
}
```

`TemplateRef`: Reference to content enclosed within the container
`ViewContainerRef`: Refers to the Container to which directive is applied

### Applying Directive to the Element
```html
<div *delayRendering="1000">
  <h1>This is the Template area</h1>
</div>
```

To create the default input, you add a @Input() and give it the same name as the directive selector
```ts
@Input() set delayRendering(delayTime: number): void { }
```

Lets add `test` input you add another @Input(). The name has to start with the directive selector and then the actual variable name, but with the first letter capitalized.

```ts
@Input() set delayRenderingTest(test: number): void { }
```

### Using variables in the template

You cannot use the delayRendering or delayRenderingTest variables in your HTML just yet. To do this you have to provide a context object. A context object can be any plain object literal

First define an interface for our directive:
```ts
export interface DirectiveContext {
  $implicit: number;
  delayRendering: number;
  delayRenderingTest: number;
}
```
These variables will be availabe in your directive / HTML. To get these values you have to use the `let x = ...` syntax. Where x can be any variable name you want. To connect `x` with the value of delayRendering you would write `let x = delayRendering`. Then you can use your `x` variable in the template like this:

```html
<div *delayRendering="10; test: 3; let x = test;">
    testValue = {{ x }}
</div>
```

The `$implicit` variable is sugared syntax as you can omit it when connecting to a variable. So `let input = $implicit`; is the same as `let input`. With this we can already get all our variables in the template

```html
<div *delayRendering="10; test: 3; let input;">
    testValue = {{ x }}
</div>
```

### Pipe

A pipe takes in data as input and transforms it to a desired output.

<img src="./assets/147465973-deccba45-bf93-41f0-961a-d5f86bcadb3b.png" width="100%">

A `pure pipe` is only called when Angular detects a change in the value or the parameters passed to a pipe. For example, any changes to a primitive input value (String, Number, Boolean, Symbol) or a changed object reference (Date, Array, Function, Object). 

An `impure pipe` is called for every change detection cycle no matter whether the value or parameters changes. i.e, An impure pipe is called often, as often as every keystroke or mouse-move.

#### Built-in Pipes
Angular provides built-in pipes for typical data transformations

| Pipe          | Description   | Example |
| ------------- | ------------- | ------------- |
| AsyncPipe     | Used to read the object from an asynchronous source  | `{{data \| async}}`
| CurrencyPipe  | Used to format the currencies  | `{{ 1234.56 \| currency:'USD' }}`
| DatePipe  | Used to format the dates  | `{{ dateVal \| date: 'fullDate' }}`
| DecimalPipe  | Used to transform the decimal numbers  | `{{ 3.141265 \| number: '1.4-4' }}`
| I18nPluralPipe  | Converts a value to a string that pluralizes the value according to locale rules  |
| I18nSelectPipe  | Used to display values according to the selection criteria  |
| KeyValuePipe  | Converts an Object or Map into an array of key value pairs  | `*ngFor="let row of rows \| keyvalue"`
| JsonPipe  | Converts an object into a JSON string  | `{{ jsonVal \| json }}`
| LowerCasePipe  | Converts a string or text to lowercase  | `{{ 'TEST' \| lowercase }}`
| PercentPipe  | Used to display percentage numbers  | `{{ 0.1236 \| percent: '2.1-2' }}`
| SlicePipe  | Used to slice an array  | `{{ [1,2,3,4,5,6] \| slice:2 }}`
| TitleCasePipe  | Converts a string or text to title case  | `{{ 'test' \| titlecase }}`
| UpperCasePipe  | Converts a string or text to uppercase  | `{{ 'test' \| uppercase }}`

#### Custom Pipes
Angular gives us the freedom to create custom pipes to encapsulate transformations that are not provided with the built-in pipes and to use them the same way as the built-in pipes.

```ts
// reverse.pipe.ts
// how to use {{text | reverseStr}}
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: "reverseStr"
})
export class ReverseStrPipe implements PipeTransform {
  transform(value: string): string {
    let newStr = "";

    for (let i = value.length - 1; i >= 0; i--) {
      newStr += value.charAt(i);
    }

    return newStr;
  }
}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## NgTemplateOutlet

### What is ngTemplateOutlet?

`ngTemplateOutlet` is a structural directive. We use it to insert a template (created by `ngTemplate`) in various sections of our DOM. For example, you can define a few templates to display an item and use them display at several places in the View and also swap that template as per the user’s choice.

### How to use ngTemplateOutlet?

In the following code, we have a template defined using the `ng-template`. The Template reference variable holds the reference the template.

The template does not render itself. We must use a structural directive to render it. That is what `ngTemplateOutlet` does

We pass the Template Reference to the `ngTemplateOutlet` directive. It renders the template. 

```html
<ng-template #listTemplate>
  <span>list</span>
</ng-template>


<!-- The following code does not render the span -->
<div *ngTemplateOutlet="listTemplate"></div>
```

### Passing data to ngTemplateOutlet

We can also pass data to the using its second property `ngTemplateOutletContext`

The following code creates a template. We name it as `listTemplate`. The `let-value` creates a local variable with the name `value`

```html
<ng-template let-value="value" #listTemplate>  
    <p>Value Received from the Parent is  {{value}}</p>
</ng-template>

<!-- We can pass any value to the value using the ngTemplateOutletContextproperty -->
<ng-container [ngTemplateOutlet]="listTemplate" [ngTemplateOutletContext] ="{value:'1000'}">
</ng-container>  
```

If you use the key `$implicit` in the context object will set its value as default for all the local variables.

```html
<ng-template let-name let-message="message" #listTemplate>  
  <p>Dear {{name}} , {{message}} </p>
</ng-template>
 
<ng-container [ngTemplateOutlet]="listTemplate" 
              [ngTemplateOutletContext] ="{$implicit:'Guest',message:'Welcome to our site'}">
</ng-container> 
```

We have not assigned anything to the `let-name` so it will take the value from the `$implicit`, which is Guest

### Passing Template to a Child Component

We can pass the entire template to a child component from the parent component. The technique is similar to passing data from parent to child component

```html
<ng-template #parentTemplate>  
  <p>
    This Template is defined in Parent. 
  </p>
</ng-template>

<child [customTemplate]="parentTemplate"></child>
```

In the Child, component receive the `parentTemplate` using the `@Input`(). And then pass it to `ngTemplateOutlet`

```ts
@Input() customTemplate: TemplateRef<HTMLElement>;
```

Use the `ViewChild` to get the access to the `parentTemplate` in the component
```ts
@ViewChild("listTemplate", { static: false }) listTemplate: TemplateRef<HTMLElement>;

public get template() {
  return this.isCard ? this.cardTemplate : this.listTemplate;
}
```
```html
<users-view [userTemplate]="template"></users-view>
```

## ngClass

`ngClass` is a directive in Angular that adds and removes CSS classes on an HTML element

```html
<!-- Basic -->
<div [ngClass]="'first second'">
<div [ngClass]="['first', 'second']">
<div [ngClass]="{first: true, second: true, third: true}">
<div [ngClass]="{'first second': true}">
```

```html
<!-- Expression -->
<div [ngClass]="val + val">
<div [ngClass]="[val]">
```

```html
<!-- Condition -->
<span [ngClass]="val > 10 ? 'red' : 'green'">{{ val }}</span>
<span [ngClass]="{ error: control.isInvalid }"></span>
<span [class.error]="control.isInvalid"></span>
```

``` ts
// ngClass as function
import { Component } from '@angular/core';

@Component({
   template: '<span [ngClass]="getClassOf(val)">{{ val }}</span>'
})
export class AppComponent { 
   getClassOf(val) {
    if (val >= 0 && val <= 5) {
      return 'low';
    } else if (val > 5 && val <= 10) {
      return 'medium';
    } else {
      return 'high'
    }
  }
} 
```

```ts
// ngClass with values in Array
import { Component } from '@angular/core';

type Val = 1 | 2 | 3;

@Component({
   template: '<span [ngClass]="classArr[val - 1]">{{ val }}</span>'
})
export class AppComponent { 
   classArr = ['first-element', 'second-element', 'third-element'];
  val: Val = 1;
} 
```

```ts
// ngClass with values in Object
import { Component } from '@angular/core';

type Val = 1 | 2 | 3;

@Component({
   template: '<span [ngClass]="classMap[val]">{{ val }}</span>'
})
export class AppComponent { 
    classMap = {
    1: 'first-element',
    2: 'second-element',
    3: 'third-element',
  }
  val: Val = 1;
} 
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Forms

**Resources**

- ["Custom Form Validators"](https://codecraft.tv/courses/angular/advanced-topics/basic-custom-validators/)
- ["Custom Form Validation in Angular"](https://www.digitalocean.com/community/tutorials/angular-custom-validation/)

- ["Reactive FormGroup validation with AbstractControl in Angular"](https://ultimatecourses.com/blog/reactive-formgroup-validation-angular-2/)

- ["ControlValueAccessor in Angular forms"](https://indepth.dev/posts/1055/never-again-be-confused-when-implementing-controlvalueaccessor-in-angular-forms/)

- ["Using ControlValueAccessor to Create Custom Form Controls in Angular"](https://www.digitalocean.com/community/tutorials/angular-custom-form-control/)

- ["Testing Dynamic Forms in Angular"](https://www.telerik.com/blogs/testing-dynamic-forms-in-angular/)

### Basic setup

```ts
import {
  ChangeDetectionStrategy,
  Component,
  Input,
  OnInit
} from "@angular/core";
import { FormControl, FormGroup, Validators } from "@angular/forms";

@Component({
  selector: "lib-form",
  templateUrl: "./form.component.html",
  styleUrls: ["./form.component.scss"],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class FormGeneralComponent implements OnInit {
  public form = new FormGroup({
    name: new FormControl("", [Validators.required]),
    description: new FormControl(undefined, [Validators.required]),
    status: new FormControl(1, [Validators.required])
  });

  get controls() {
    return this.form.controls;
  }

  public ngOnInit() {
    this.initializeFormValues();
  }

  // reset form
  public reset() {
    this.form.reset();
  }

  // populate form values
  public initializeFormValues() {
    this.form.patchValue({
      name: "name",
      description: "description",
      status: 2
    });
  }

  // GET form values
  public onSubmit() {
    this.form.getRawValue();
  }
}
```

### Nested Forms

The best approach is to create a stateful parent component and many children stateless components.

Parent component needs to be dedicated for particular form. Child components can be reused everywhere many times.

#### Parent component rules:

- stateful
- creates and holds form definition
- emits form state (value, valid, pristine) on every form change
- holds custom validation logic

#### Children components rules:

- stateless
- receives form parts (nested FormGroups) from parent
- no custom validation logic

In above scenario children components are "reusable views" without any validation logic. It will always comes from parent.

#### Parent component

```ts
// parent-form.componnet.ts
import { Component, ChangeDetectionStrategy } from "@angular/core";
import { FormGroup } from "@angular/forms";

@Component({
  selector: "bb-nested-form",
  templateUrl: "./nested-form.component.html",
  styleUrls: ["./nested-form.component.scss"],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class NestedFormComponent {
  public form = new FormGroup({
    general: new FormGroup({})
  });

  get controls() {
    return this.form.controls;
  }

  public onSubmit() {
    if (this.form.valid) {
      const formValue = this.form.getRawValue();
      console.log(formValue);
    }
  }
}
```

```html
<!-- parent-form.component.html -->
<div class="row">
  <div class="col-6">
    <label>General:</label>
    <bb-form-general [parentForm]="controls.general"></bb-form-general>
  </div>
</div>

<button (click)="onSubmit()">Submit form</button>
```

#### Child component

```ts
// child-form.component
import {
  Component,
  OnInit,
  Input,
  ChangeDetectionStrategy
} from "@angular/core";
import { FormGroup, FormControl, Validators } from "@angular/forms";
import { FormsService } from "../../../services/forms.service";

@Component({
  selector: "bb-form-general",
  templateUrl: "./form-general.component.html",
  styleUrls: ["./form-general.component.scss"],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class FormGeneralComponent implements OnInit {
  @Input() public parentForm!: FormGroup;

  public form = new FormGroup({
    name: new FormControl("", [Validators.required]),
    description: new FormControl(undefined, [Validators.required])
  });

  get controls() {
    return this.form.controls;
  }

  constructor(public formsService: FormsService) {}

  public ngOnInit() {
    this.formsService.addGroupToParentForm(this.parentForm, this.form);
  }
}
```

```html
<form [formGroup]="form">
  <div class="row">
    <div class="col-12">
      <label>Name</label>
      <input type="text" formControlName="name" />
    </div>

    <div class="col-12">
      <label>Description</label>
      <textarea type="text" formControlName="description"></textarea>
    </div>
  </div>
</form>
```

#### Form service

```ts
// forms.service.ts
import { Injectable } from "@angular/core";
import { FormGroup } from "@angular/forms";

@Injectable()
export class FormsService {
  public addGroupToParentForm(parentForm: FormGroup, group: FormGroup) {
    for (const [key, control] of Object.entries(group.controls)) {
      parentForm.addControl(key, control);
    }
    group.setParent(parentForm);
  }
}
```

### Dynamic Forms

```ts
// dynamic-form.component.ts
import { Component } from "@angular/core";
import { FormGroup, FormArray, FormControl, Validators } from "@angular/forms";

@Component({
  selector: "bb-dynamic-form",
  templateUrl: "./dynamic-form.component.html",
  styleUrls: ["./dynamic-form.component.scss"],
})
export class DynamicFormComponent {
  public form: FormGroup = new FormGroup({
    userName: new FormControl("", [Validators.required]),
    timeRanges: new FormArray([]),
  });

  get controls() {
    return this.form.controls;
  }

  get timeRangeControls() {
    return this.form.get("timeRanges") as FormArray;
  }

  public addNewTimeRange() {
    this.timeRangeControls.push(this.singleRange());
  }

  public deleteTimeRange(i: number) {
    this.timeRangeControls.removeAt(i);
  }

  private singleRange() {
    return new FormGroup({
      startDate: new FormControl("", [Validators.required]),
      endDate: new FormControl("", [Validators.required]),
    });
  }

  public onSubmit() {
    console.log(this.form.getRawValue());
  }
}
```

```html
<form [formGroup]="form" novalidate>
  <div class="row">
    <div class="col-12">
      <label>User Name</label>
      <input type="text" formControlName="userName" />
      <l9-validation-message [control]="controls.userName"></l9-validation-message>
    </div>

    <div class="col-12">
      <h4>Time Ranges</h4>
      <ng-container formArrayName="timeRanges">
        <div *ngFor="let _ of timeRangeControls.controls; let i = index" class="row">
          <ng-container [formGroupName]="i">
            <!-- Start Date -->
            <div class="col-5">
              <label>Start Date</label>
              <input type="date" formControlName="startDate" />
              <l9-validation-message [control]="timeRangeControls.at(i).get('startDate')"></l9-validation-message>
            </div>

            <!-- End Date -->
            <div class="col-5">
              <label>End Date</label>
              <input type="date" formControlName="endDate" />
              <l9-validation-message [control]="timeRangeControls.at(i).get('endDate')"></l9-validation-message>
            </div>

            <div class="col-2">
              <button (click)="deleteTimeRange(i)">-</button>
            </div>
          </ng-container>
        </div>

        <div class="col-12">
          <button (click)="addNewTimeRange()">+</button>
        </div>
      </ng-container>
    </div>
  </div>
</form>

<button (click)="onSubmit()">Submit form</button>
```

### Custom FormGroup Validator

```ts

// form.component.ts

import { atLeastOneRequiredValidator } from "./validators.ts";

public form = new FormGroup({
  programName: new FormGroup(
    {
        program: new FormControl(""),
        switch: new FormControl(""),
        intervention: new FormControl(""),
    },
    { validators: atLeastOneRequiredValidator() },
  ),
});
```

```ts
// validators.ts
import { ValidatorFn, FormGroup, ValidationErrors } from "@angular/forms";

export const atLeastOneRequiredValidator = (): ValidatorFn => {
  return (group: FormGroup): ValidationErrors => {
    const control1 = group.controls["program"];
    const control2 = group.controls["switch"];
    const control3 = group.controls["intervention"];

    if (
      control1.value === "" &&
      control2.value === "" &&
      control3.value === ""
    ) {
      return { empty: true };
    }

    return null;
  };
};
```

### Custom FormControl Validator

```ts
// form.component.ts
import { PhoneNumberValidators } from "./validators.ts";

public form = new FormGroup({
  phone: ['', [PhoneNumberValidators.phoneValidator()]]
});
```

```ts
// validators.ts
export class PhoneNumberValidators {
  static phoneValidator(): ValidatorFn {
    return (control: AbstractControl): ValidationErrors | null => {
      if (control.value && control.value.length != 10) {
        return { phoneNumberInvalid: true };
      }
    return null;
    };
  }
}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

### ControlValueAccessor

When creating forms in Angular, sometimes you want to have an input that isn’t a standard text input, select, or checkbox. By implementing the `ControlValueAccessor` interface and registering the component as a `NG_VALUE_ACCESSOR`, you can integrate your custom form control seamlessly into template driven or reactive forms just as if it were a native input!

Any component or directive can be turned into `ControlValueAccessor` by implementing the ControlValueAccessor interface and registering itself as an `NG_VALUE_ACCESSOR` provider.

```ts
interface ControlValueAccessor {
  writeValue(obj: any): void
  registerOnChange(fn: any): void
  registerOnTouched(fn: any): void
  setDisabledState(fn: any): void
  ...
}
```

Write a value to the input - `writeValue`

Register a function to tell Angular when the value of the input changes - `registerOnChange`

Register a function to tell Angular when the input has been touched - `registerOnTouched`

Disable the input - `setDisabledState`

These four things make up the `ControlValueAccessor` interface, the bridge between a form control and a native element or custom input component. Once our component implements that interface, we need to tell Angular about it by providing it as a `NG_VALUE_ACCESSOR` so that it can be used.

Here is the diagram that demonstrates an interaction:

<img src="./assets//1_wvjxZqL4ZZVGmsh3VFV2Ew.jpeg" width="684" />

#### Implementing custom value accessor

Implementing a custom value accessor is not difficult. It requires 2 simple steps:

- registering a `NG_VALUE_ACCESSOR` provider
- implementing `ControlValueAccessor` interface methods

`NG_VALUE_ACCESSOR` provider specifies a class that implements `ControlValueAccessor` interface and is used by Angular to setup synchronization with `formControl`. It’s usually the class of the component or directive that registers the provider. All form directives inject value accessors using the token `NG_VALUE_ACCESSOR` and then select a suitable accessor. If there is an accessor which is not built-in or `DefaultValueAccessor` it is selected. Otherwise Angular picks the default accessor if it’s provided. And there can be no more than one custom accessor defined for an element.

So let’s first define the provider:

```ts

export const VALUE_ACCESSOR: Provider = [
  {
    provide: NG_VALUE_ACCESSOR,
    useExisting: forwardRef(() => CustomFormComponent),
    multi: true,
  }
];

@Component({
  selector: '',
  providers: [...VALUE_ACCESSOR],
  ...
})
export class CustomFormComponent implements ControlValueAccessor {...}
```

Once we defined a provider let’s implement ControlValueAccessor interface:

```ts
export class CustomFormComponent implements ControlValueAccessor {
  // Allow the input to be disabled, and when it is make it somewhat transparent.
  @Input() disabled = false;

  dataPropery: boolean[] = Array(5).fill(false);
  // Function to call when the rating changes.
  onChange = (rating: number) => {};
  // Function to call when the input is touched (when a star is clicked).
  onTouched = () => {};
  // Allows Angular to update the model (rating).
  // Update the model and changes needed for the view here.
  writeValue(rating: number): void {
    this.dataPropery = this.dataPropery.map((_, i) => rating > i);
    this.onChange(this.value);
  }
  // Allows Angular to register a function to call when the model (rating) changes.
  // Save the function as a property to call later here.
  registerOnChange(fn: (rating: number) => void): void {
    this.onChange = fn;
  }
  // Allows Angular to register a function to call when the input has been touched.
  // Save the function as a property to call later here.
  registerOnTouched(fn: () => void): void {
    this.onTouched = fn;
  }
  // Allows Angular to disable the input.
  setDisabledState(isDisabled: boolean): void {
    this.disabled = isDisabled;
  }
}
```


### Testing Forms

The first step is to set up the test bed for the component. Angular already provides a boilerplate for testing the component, and we’ll simply extend that:

```ts
import { async, ComponentFixture, TestBed } from '@angular/core/testing';
    import { ReactiveFormsModule } from '@angular/forms';
    import { DynamicFormComponent } from './dynamic-form.component';
    
    describe('DynamicFormComponent', () => {
      let component: DynamicFormComponent;
      let fixture: ComponentFixture<DynamicFormComponent>;
    
      beforeEach(async(() => {
        TestBed.configureTestingModule({
          declarations: [ DynamicFormComponent ],
          imports: [ ReactiveFormsModule ],
        })
        .compileComponents();
      }));
    
      beforeEach(() => {
        fixture = TestBed.createComponent(DynamicFormComponent);
        component = fixture.componentInstance;
        fixture.detectChanges();
      });
    
      it('should create', () => {
        expect(component).toBeTruthy();
      });
    });
```

#### We’ll be testing our form using the following cases:

1) `Form rendering`: here, we’ll check if the component generates the correct input elements when provided a formConfig array.
2) `Form validity`: we’ll check that the form returns the correct validity state
3) `Input validity`: we’ll check if the component responds to input in the view template
4) `Input errors`: we’ll test for errors on the required input elements.

#### Form Rendering
For this test, we’ll we’ll test that the component renders the correct elements.

```ts
it('should render input elements', () => {
  const compiled = fixture.debugElement.nativeElement;
  const addressInput = compiled.querySelector('input[id="address"]');
  const nameInput = compiled.querySelector('input[id="name"]');

  expect(addressInput).toBeTruthy();
  expect(nameInput).toBeTruthy();
});
```
#### Form validity
For this test, we’ll check for the validity state of the form after updating the values of the input elements. For this test, we’ll update the values of the form property directly without accessing the view.

```ts
it('should test form validity', () => {
  const form = component.form;
  expect(form.valid).toBeFalsy();

  const nameInput = form.controls.name;
  nameInput.setValue('John Peter');

  expect(form.valid).toBeTruthy();
})
```

For this test, we’re checking if the form responds to the changes in the control elements. When creating the elements, we specified that the name element is required. This means the initial validity state of the form should be `INVALID`, and the valid property of the form should be false.

Next, we update the value of the name input using the `setValue` method of the form control, and then we check the validity state of the form. After providing the required input of the form, we expect the form should be valid.

#### Input validity
Next we’ll check the validity of the input elements. The name input is required, and we should test that the input acts accordingly. Open the spec file and add the spec below to the test suite:

```ts
it('should test input validity', () => {
  const nameInput = component.form.controls.name;
  const addressInput = component.form.controls.address;

  expect(nameInput.valid).toBeFalsy();
  expect(addressInput.valid).toBeTruthy();

  nameInput.setValue('John Peter');
  expect(nameInput.valid).toBeTruthy();
})
```

In this spec, we are checking the validity state of each control and also checking for updates after a value is provided.

Since the name input is required, we expect its initial state to be invalid. The address isn’t required so it should be always be valid. Next, we update the value of the name input, and then we test if the valid property has been updated.

#### Input errors

In this spec, we’ll be testing that the form controls contain the appropriate errors; the name control has been set as a required input. We used the `Validators` class to validate the input. The form control has an errors property which contains details about the errors on the input using key-value pairs.

```ts
it('should test input errors', () => {
  const nameInput = component.form.controls.name;
  expect(nameInput.errors.required).toBeTruthy();

  nameInput.setValue('John Peter');
  expect(nameInput.errors).toBeNull();
});
```
First, we get the name form control from the form form group property. We expect the initial errors object to contain a required property, as the input’s value is empty. Next, we update the value of the input, which means the input shouldn’t contain any errors, which means the errors property should be null.

If all tests are passing, it means we’ve successfully created a form. 

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Routing

**Resources**

- ["Angular Component Reuse Strategy"](https://medium.com/@juliapassynkova/angular-2-component-reuse-strategy-9f3ddfab23f5/)
- ["How To Use Route Resolvers in Angular"](https://www.digitalocean.com/community/tutorials/angular-route-resolvers)
- ["Componentless Route"](https://netbasal.com/implementing-auth-guard-with-componentless-route-in-angular-b50a21f3bd77)

### Componentless Route

`Componentless routes` are useful when the same configuration apply to all child routes.

For example guards, resolvers, params, etc.,

```ts
const routes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
  {
    path: '',
    canActivateChild: [AuthGuard],
    resolve: {
      token: TokenNeededForBothMessagsAndContacts
    },
    children: [
      {
        path: 'todos',
        component: TodosComponent
      },
      {
        path: 'blog',
        component: BlogComponent
      }
    ]
  }
]
```

And that’s how it looks with lazy-load routes:

```ts
const routes : Routes = [
  {
    path: '',
    pathMatch: 'full',
    loadChildren: './home/home.module#HomeModule'
  },
  {
    path: '',
    canActivateChild: [AuthGuard],
    children: [
      {
        path: 'about',
        loadChildren: './about/about.module#AboutModule'
      },
      {
        path: 'posts',
        loadChildren: './posts-page/posts-page.module#PostsPageModule'
      },
    ]
  }
]
```

### Route Resolvers

`Route Resolver` allows you to get data before navigating to the new route.

### Basic Implementation

```ts
import { Injectable } from '@angular/core';
import { Resolve } from '@angular/router';

import { Observable, of } from 'rxjs';
import { delay } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class NewsResolver implements Resolve<Observable<string>> {
  resolve(): Observable<string> {
    return of('Resolver').pipe(delay(1000));
  }
}
```

### Configuring Routes

```ts
{
  path: 'top',
  component: Component,
  resolve: { message: NewsResolver }
}
```

### Accessing the Resolved Data in the Component
In the component, you can access the resolved `data` using the data property of `ActivatedRoute’s` snapshot object

```ts
import { ActivatedRoute } from '@angular/router';

@Component({ ... })
...
constructor(private route: ActivatedRoute) {}

ngOnInit(): void {
  console.log(this.route.snapshot.data);
}
```
### Resolving Data from an API

First, add the `HttpClientModule` to `app.module.ts`

Then, create a new service:

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class NewsService {
  constructor(private http: HttpClient) { }

  getTopPosts() {
    return this.http.get("https://hacker-news.firebaseio.com/v0/topstories.json");
  }
}
```

And now you can replace the string code in `NewsResolver` with `NewsService`

```ts
import { Injectable } from '@angular/core';
import { Resolve } from '@angular/router';
import { Observable } from 'rxjs';

import { NewsService } from './news.service';

export class NewsResolver implements Resolve<any> {
  constructor(private newsService: NewsService) {}

  resolve(): Observable<any> {
    return this.newsService.getTopPosts();
  }
}
```
#### Accessing Route Parameters

```ts
import { Injectable } from '@angular/core';
import { Resolve, ActivatedRouteSnapshot } from '@angular/router';
import { Observable } from 'rxjs';

import { NewsService } from './news.service';

@Injectable({
  providedIn: 'root'
})
export class PostResolver implements Resolve<any> {
  constructor(private newsService: NewsService) {}

  resolve(route: ActivatedRouteSnapshot): Observable<any> {
    return this.newsService.getPost(route.paramMap.get('id'));
  }
}
```

#### Handling Errors

```ts
import { Injectable } from '@angular/core';
import { Resolve } from '@angular/router';

import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators';

import { NewsService } from './news.service';

@Injectable()
export class NewsResolver implements Resolve<any> {
  constructor(private newsService: NewsService) {}

  resolve(): Observable<any> {
    return this.newsService.getTopPosts().pipe(catchError(() => {
      return of('data not available at this time');
    }));
  }
}
```

### Custom RouteReuseStrategy

RouteReuseStrategy decides on whether the router should store the current route when deactivating it or whether the router should restore it when the user re-activates it.

```ts
// app.module.ts
import { RouteReuseStrategy } from "@angular/router";
import { CustomReuseStrategy } from "./router-reuse.strategy.ts";

@NgModule({
  declarations: [],
  imports: [],
  providers: [{ provide: RouteReuseStrategy, useClass: CustomReuseStrategy }],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

````ts
// router-reuse.strategy.ts
import { Injectable } from "@angular/core";
import {
  RouteReuseStrategy,
  ActivatedRouteSnapshot,
  DetachedRouteHandle
} from "@angular/router";
/**
 * Based on Angular `DefaultRouteReuseStrategy`.
 * Reuses routes as long as their route config is the same OR until future route data has pattribute `noReuse: true`
 *
 * @example ```json
 *   {
 *       path: "overview",
 *       component: OverviewComponent,
 *        data: {
 *            noReuse: true,
 *        },
 *    },
 * ```
 */
@Injectable()
export class CustomReuseStrategy implements RouteReuseStrategy {
  public shouldDetach(route: ActivatedRouteSnapshot): boolean {
    return false;
  }

  public store(
    route: ActivatedRouteSnapshot,
    detachedTree: DetachedRouteHandle
  ): void {}

  public shouldAttach(route: ActivatedRouteSnapshot): boolean {
    return false;
  }

  public retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle | null {
    return null;
  }

  public shouldReuseRoute(
    future: ActivatedRouteSnapshot,
    curr: ActivatedRouteSnapshot
  ): boolean {
    if (future.data && Boolean(future.data.noReuse)) {
      return !future.data.noReuse;
    }
    return future.routeConfig === curr.routeConfig;
  }
}
````

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Unit testing

Tests are vital when programming because they help detect issues within your codebase that otherwise would have been missed. Writing proper tests reduces the overhead of manually testing functionality in the view or otherwise.

**Resources**

- ["How to test OnPush components"](https://medium.com/@juliapassynkova/how-to-test-onpush-components-c9b39871fe1e/)

### How to test OnPush components

Angular `ChangeDetectionStrategy.OnPush` is a suggested way to improve the performance of Angular applications. When a component’s `changeDetection` is `OnPush` only input ref change and output’s call will trigger the change detection mechanism for the component and all its children. However, Angular has a defect that affects testing of such components. This post shows how `TestBed.overrideComponent` helps to overcome this defect.

#### Component code
Here is a simple component with ChangeDetectionStrategy.OnPush

```ts
@Component({
  selector: 'test',
  template: `test id = <span>{{id}}</span>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class TestComponent {
  @Input() id: number;
}
```

```ts
import {Component} from '@angular/core';
import {TestBed, ComponentFixture, async} from '@angular/core/testing';
import {TestComponent} from 'app/app.component';

describe('TestComponent', () => {
  let fixture: ComponentFixture<TestComponent>, comp: TestComponent, element: HTMLElement;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [TestComponent]
    });
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(TestComponent);
    comp = fixture.componentInstance;
    element = fixture.debugElement.nativeElement;
  });

  it('can modify the id option', async(() => {
    comp.id = 1;
    fixture.detectChanges();

    comp.id = 2;
    fixture.detectChanges();

    expect(fixture.nativeElement
      .querySelector('span').textContent).toContain(2);
  }));
});
```

It fails due to a defect in Angular that fixture.detectChanges() works ONLY the first time with ChangeDetectionStrategy.OnPush and karma reports

#### The solution

Angular `TestBed` provides a way to override a component metadata with `overrideComponent`. Therefore, we can override `OnPush` with Default change detection just for testing and hooray test works!

```ts
TestBed.overrideComponent(TestComponent, {
  set: new Component({
    selector: 'test',
    template: `test id = <span>{{id}}</span>`,
    changeDetection: ChangeDetectionStrategy.Default
  })
});
```

### How to test Custom Form Control Validator

#### Validator

```ts
// password-control.validator.ts
import { AbstractControl, ValidationErrors, ValidatorFn } from "@angular/forms";

const pattern = "^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9]).{7,}";

export class PasswordValidators {
  static isPasswordInCorrectFormatValidator(): ValidatorFn {
    return (control: AbstractControl): ValidationErrors | null => {
      const reg = new RegExp(pattern);
      if (control.value && !reg.test(String(control.value))) {
        return { error: true };
      }

      return null;
    };
  }
}
```

#### Unit tests

```ts
// password-control.validator.spec.ts
import { FormControl } from '@angular/forms';
import { PasswordValidators } from './password-control.validator';

/*
A minimum of 7 characters
At least one UPPERCASE letter
At least one lowercase letter
At least one number
*/

const TEST_CASES = [
  { test_case: 'string length is less than 7 characters', value: '12345', result: { error: true } },
  { test_case: 'string do not have UPPERCASE letter', value: '12345qwe', result: { error: true } },
  { test_case: 'string do not have lowercase letter', value: '12345WWW', result: { error: true } },
  { test_case: 'string do not have one number', value: 'qweqweWWW', result: { error: true } },
  { test_case: 'string has correct value', value: '123qweQW', result: null },
];

describe('PasswordValidators', () => {
  const isPasswordInCorrectFormatValidator = PasswordValidators.isPasswordInCorrectFormatValidator();
  const control = new FormControl('input');

  TEST_CASES.forEach(({ test_case, value, result }) => {
    it(`should return ${result} if input ${test_case}`, () => {
      control.setValue(value);
      expect(isPasswordInCorrectFormatValidator(control)).toEqual(result);
    });
  });
});

```

### How to test Pipes

#### Pipe

```ts
// string-to-number.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'toNumber' })
export class ToNumberPipe implements PipeTransform {
  transform(value: string | number): number {
    if (!value) return NaN;

    if (typeof value === 'string' && value.trim().length === 0) {
      return NaN;
    }
    return Number(value);
  }
}
```

#### Unit tests

```ts
// string-to-number.pipe.spec.ts
import { ToNumberPipe } from './string-to-number.pipe';

const TEST_CASES = [
  { value: 12, result: 12 },
  { value: 12.2, result: 12.2 },
  { value: '12', result: 12 },
  { value: '', result: NaN },
  { value: ' ', result: NaN },
  { value: '12a', result: NaN },
  { value: 'vv12', result: NaN },
];

describe('ToNumberPipe', () => {
  const pipe = new ToNumberPipe();
  it('should create a pipe instance', () => expect(pipe).toBeTruthy());

  TEST_CASES.forEach(({ value, result }) => {
    it(`should match the ${value} with to ${result}`, () => {
      expect(pipe.transform(value)).toEqual(result);
    });
  });
});
```

### How to test Presentational components
```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';
 
@Component({
  selector: 'test-example',
  template: `
    <h1 class="title">Test Components</h1>
    <span *ngIf="isSubTitleVisible" class="sub-title"> sub title </span>
    <button data-role="test-action-button" (click)="onClick()">click me</button>
    <button data-role="test-action-with-param-button" (click)="onClickWithParam('string param')">
      click me with data
    </button>
  `,
})
export class TestComponent {
  @Input() public isSubTitleVisible = false;
  @Output() public click = new EventEmitter<{ someData: string }>();
  @Output() public clickWithParam = new EventEmitter<string>();
 
  public onClick() {
    this.click.emit({ someData: 'test string' });
  }
 
  public onClickWithParam(event: string) {
    this.clickWithParam.emit(event);
  }
}
```
#### Unit tests

```ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';

import { TestComponent } from './test.component';

describe('TestComponent', () => {
  let component: TestComponent;
  let fixture: ComponentFixture<TestComponent>;
  let title: HTMLElement;
  let actionButton: HTMLButtonElement;
  let actionButtonWithParam: HTMLButtonElement;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [],
      declarations: [TestComponent],
    });
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(TestComponent);
    component = fixture.debugElement.componentInstance;

    fixture.detectChanges();

    title = fixture.debugElement.query(By.css('.title')).nativeElement as HTMLElement;
    actionButton = fixture.debugElement.query(By.css("button[data-role='test-action-button']"))
      .nativeElement as HTMLButtonElement;
    actionButtonWithParam = fixture.debugElement.query(By.css("button[data-role='test-action-with-param-button']"))
      .nativeElement as HTMLButtonElement;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should contain title and sub_title', () => {
    component.isSubTitleVisible = true;
    fixture.detectChanges();
    const sub_title = fixture.debugElement.query(By.css('.sub-title')).nativeElement as HTMLElement;

    expect(title).toBeTruthy();
    expect(sub_title).toBeTruthy();

    expect(title.textContent?.trim()).toEqual('Test Components');
    expect(sub_title.textContent?.trim()).toMatch('sub title');
  });

  it('should emit when button is clicked', () => {
    spyOn(component.click, 'emit');
    actionButton.click();
    expect(component.click.emit).toHaveBeenCalled();
    expect(component.click.emit).toHaveBeenCalledWith({ someData: 'test string' });
  });

  it('should emit when button with param is clicked', () => {
    spyOn(component.clickWithParam, 'emit');
    actionButtonWithParam.click();
    expect(component.clickWithParam.emit).toHaveBeenCalled();
    expect(component.clickWithParam.emit).toHaveBeenCalledWith('string param');
  });

  it('should check visibility of subTitle based on isSubTitleVisible property', () => {
    component.isSubTitleVisible = false;
    fixture.detectChanges();
    const subTitleHidden = fixture.debugElement.query(By.css('.sub-title'));

    expect(subTitleHidden).toBeNull();

    component.isSubTitleVisible = true;
    fixture.detectChanges();

    const subTitleVisible = fixture.debugElement.query(By.css('.sub-title')).nativeElement as HTMLElement;

    expect(subTitleVisible).toBeTruthy();
    expect(subTitleVisible.textContent?.trim()).toMatch('sub title');
  });
});
```

### How to test HTTP service

```ts
// post.service.ts
import { HttpClient, HttpHeaders } from "@angular/common/http";
import { Injectable } from "@angular/core";
import { Observable, of } from "rxjs";
import { catchError } from "rxjs/operators";

export interface Post {
  id: string;
  title: string;
  body: string;
}

@Injectable({
  providedIn: "root",
})
export class PostsService {
  url = "https://jsonplaceholder.typicode.com";

  httpOptions = {
    headers: new HttpHeaders({ "Content-Type": "application/json" }),
  };

  constructor(private http: HttpClient) {}

  getAllPosts(): Observable<Post[]> {
    return this.http.get<Post[]>(`${this.url}/posts`).pipe(catchError(this.handleError<Post[]>("getAllPosts", [])));
  }

  getPostById(id: string): Observable<Post> {
    return this.http
      .get<Post>(`${this.url}/posts/${id}`)
      .pipe(catchError(this.handleError<Post>(`getPostById id=${id}`)));
  }

  updatePost(post: Post): Observable<any> {
    return this.http
      .put(`${this.url}/posts`, post, this.httpOptions)
      .pipe(catchError(this.handleError<any>(`updatePost`)));
  }

  addPost(post: Post): Observable<Post> {
    return this.http
      .post<Post>(`${this.url}/posts`, post, this.httpOptions)
      .pipe(catchError(this.handleError<Post>(`addPost`)));
  }

  deletePost(post: Post): Observable<Post> {
    return this.http
      .delete<Post>(`${this.url}/posts/${post.id}`, this.httpOptions)
      .pipe(catchError(this.handleError<Post>(`deletePost`)));
  }

  private handleError<T>(operation = "operation", result?: T) {
    return (error: any): Observable<T> => {
      console.error(`${operation} failed: ${error.message}`);

      return of(result as T);
    };
  }
}
```

#### Unit tests
```ts
// post.service.spec.ts
import { TestBed } from "@angular/core/testing";
import { Post, PostsService } from "./post.service";
import { HttpClientTestingModule, HttpTestingController } from "@angular/common/http/testing";
import { postsMock } from "../mocks/posts.mock";

describe("[SERVICES]: PostsService", () => {
  let service: PostsService;
  let httpController: HttpTestingController;
  let url = "https://jsonplaceholder.typicode.com";

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [PostsService],
    });

    service = TestBed.inject(PostsService);
    httpController = TestBed.inject(HttpTestingController);
  });

  it("should be initialized with default state", () => {
    expect(service).toBeTruthy();
  });

  it("should call getAllPosts and return an array of Posts", () => {
    service.getAllPosts().subscribe((res) => {
      expect(res).toEqual(postsMock);
    });

    const req = httpController.expectOne({ method: "GET", url: `${url}/posts` });

    req.flush(postsMock);
  });

  it("should call getPostById and return the appropriate Book", () => {
    const id = "1";

    service.getPostById(id).subscribe((data) => {
      expect(data).toEqual(postsMock[0]);
    });

    const req = httpController.expectOne({ method: "GET", url: `${url}/posts/${id}` });

    req.flush(postsMock[0]);
  });

  it("should call updatePost and return the updated Post from the API", () => {
    const updatedPost: Post = { id: "1", title: "New title", body: "Author 1" };

    service.updatePost(postsMock[0]).subscribe((data) => {
      expect(data).toEqual(updatedPost);
    });

    const req = httpController.expectOne({ method: "PUT", url: `${url}/posts` });

    req.flush(updatedPost);
  });

  it("should call addPost and return the add Post from the API", () => {
    service.addPost(postsMock[0]).subscribe((data) => {
      expect(data).toEqual(postsMock[0]);
    });

    const req = httpController.expectOne({ method: "POST", url: `${url}/posts` });

    req.flush(postsMock[0]);
  });

  it("should call addPost and return the add Post from the API", () => {
    service.deletePost(postsMock[1]).subscribe((data) => {
      expect(data).toEqual(postsMock[1]);
    });

    const req = httpController.expectOne({ method: "DELETE", url: `${url}/posts/4` });

    req.flush(postsMock[1]);
  });
});
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Error Handling

Learn how to automatically catch all errors in a web application written in Angular and process them accordingly

**Resources**

- ["Global Error Handling in Angular"](https://pkief.medium.com/global-error-handling-in-angular-ea395ce174b1)

- ["Error Handling & Angular"](https://medium.com/@aleixsuau/error-handling-angular-859d529fa53a)

### Errors, Exceptions & CallStack

When an Error happens, an Exception is thrown at the current point of execution and it will remove (unwind) every function of the CallStack until the exception is handled by a try/catch block. Then, the control will continue from the statement right after the catch block.

`If no try/catch block is found, the Exception will remove all the functions of the CallStack, crashing completely our app`.

```ts

fireError() {
  const shthppns = r; // r is not defined === ReferenceError
  console.log("I won't be logged");
}

fireErrorWithNet() {
  try {
    const shthppns = r; // r is not defined === ReferenceError
  } catch (error) {
    console.log('> Error is handled: ', error.name);
  }
  console.log('> And Control continues from this statement');
}
```

As you can see in the code, the try/catch block prevents the app from crashing and lets the program continue right below the catch.

#### The Client Errors

Since we as the developers don’t know where and when such an error could occur, it is important to catch all occurring errors at a central location

A client error should contain:
- name (ie: `ReferenceError`).
- message (ie: `X is not defined`).

And in most modern browsers: `fileName`, `lineNumber` and `columnNumber` where the error happened, and stack (last X functions called before the error).

#### The Server Errors

It is clear that the error is coming from the back end, there is a need to take care of the error handling for every single request to the back end. Again, it is better to handle these errors in a centralized location so that the user is presented with consistent error messages and also to avoid forgetting to intercept errors.

A server error might contain:
- `status` (or code): Code status starting with 4 (4xx…).
- `name`: The name of the error (ie: `HttpErrorResponse`).
- `message`: Explanation message (ie: Http failure response for…).


### Global Error Handler

By default, Angular comes with its own `ErrorHandler` that intercepts all the Errors that happen in our app and logs them to the console, preventing the app from crashing.
We can modify this default behavior by creating a new class that implements the `ErrorHandler`
Inside the `ErrorHandler`, we can check which kind of error it is

```ts
// global-error-handler.ts
import { ErrorHandler, Injectable, Injector } from "@angular/core";
import { HttpErrorResponse } from "@angular/common/http";
import { environment } from "@env/environment";

@Injectable()
export class GlobalErrorHandler implements ErrorHandler {
  constructor(private injector: Injector) {}

  handleError(error: Error | HttpErrorResponse) {
    if (error instanceof HttpErrorResponse) {
      // Server error happened
      this.handleServerError(error);
    } else {
      // Client Error Happend
      this.handleClientError(error);
    }
  }

  // Customize the default server error handler here if needed
  private handleServerError(error: HttpErrorResponse) {
    if (!navigator.onLine) {
      // No Internet connection
      alert("No Internet Connection");
    }

    if (!environment.production) {
      // Http Error
      // Show notification to the user
      console.error("Request error", error);
      alert(`${error.status} - ${error.message}`);
    }
  }

  private handleClientError(error: Error) {
    console.error(error);
  }
}
```

Because the best Error is the one that never happens, we could improve our error handling using an `HttpInterceptor` that would intercept all the server calls and retry them X times before throw an Error

```ts
// http-error.interceptor.ts
import { Injectable } from "@angular/core";
import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest } from "@angular/common/http";
import { Observable } from "rxjs";
import { finalize, retry } from "rxjs/operators";

import { LoaderService } from "@core/services/loader.service";

@Injectable({
  providedIn: "root",
})
export class HttpErrorInterceptor implements HttpInterceptor {
  constructor(public loaderService: LoaderService) {}

  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    this.loaderService.display(true);
    // If the call fails, retry until 2 times before throwing an error
    return next.handle(request).pipe(
      retry(2),
      finalize(() => {
        this.loaderService.display(false);
      }),
    );
  }
}
```

For Error Handling we need to register two `providers`. The first one is responsible for the general error handling, which catches all errors occurring within our application. The second provider is an HTTP interceptor, which is called for every interaction with the back end. The multi property must always be set to `true` in this case, since the `HTTP_INTERCEPTORS` injection token can potentially be assigned to several classes.

```ts
// core.module.ts
@NgModule({
  imports: [ ... ],
  declarations: [ ... ],
  providers: [
    {
      provide: ErrorHandler,
      useClass: GlobalErrorHandler,
    },
    {
      provide: HTTP_INTERCEPTORS,
      useClass: HttpErrorInterceptor,
      multi: true,
    },
  ]
})
```


<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## JWT Token Interceptor

```ts
import { Injectable, Injector, ErrorHandler } from "@angular/core";
import {
  HttpEvent,
  HttpInterceptor,
  HttpHandler,
  HttpResponse,
  HttpRequest,
  HttpErrorResponse
} from "@angular/common/http";
import { Observable, of } from "rxjs";
import { tap } from "rxjs/operators";

@Injectable()
export class TokenInterceptor implements HttpInterceptor {
  constructor() {}

  intercept(
    request: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    request = request.clone({
      setHeaders: {
        Authorization: `Bearer token`
      }
    });

    return next.handle(request);
  }
}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Dynamic Components

Component templates are not always fixed. An application may need to load new components at runtime.

This cookbook shows you how to use `ComponentFactoryResolver` to add components dynamically.

In the component, we are creating a template element. We are also using the hash symbol (#) to declare a reference variable named `dynamicLoadDevicesComponent`. The template element is the place, or in the Angular world, the container.

```html
<template #dynamicLoadDevicesComponent></template>
```

We can get a reference to the template element with the ViewChild decorator that also takes a local variable as a parameter

```ts
@ViewChild("dynamicLoadDevicesComponent", {read: ViewContainerRef, static: true }) private entry: ViewContainerRef;
```

Before we proceed to the createComponent() method, we need to add one more service

```ts
constructor(private resolver: ComponentFactoryResolver) {}
```

The `ComponentFactoryResolver` service exposes one primary method, `resolveComponentFactory`.
The `resolveComponentFactory()` method takes a component and returns a `ComponentFactory`.
You can think of `ComponentFactory` as an object that knows how to create a component.
As you can see the `ComponentFactory` exposes the `create()` method that will be used by the container ( ViewContainerRef ) internally.

```ts
private createComponent() {
  this.entry.clear();
  const factory = this.resolver.resolveComponentFactory(RedDeviceComponent);
  this.componentRef = this.entry.createComponent(factory);
}
```

Let’s explain what is happening piece by piece.

```ts
this.entry.clear();
```

Every time we need to create the component we need to remove the previous view, otherwise, it will append more components to the container. (not required if you need multiple components)

```ts
const factory = this.resolver.resolveComponentFactory(RedDeviceComponent);
this.componentRef = this.entry.createComponent(factory);
```

The resolveComponentFactory() method takes a component and returns the recipe for how to create a component.

And don’t forget to destroy the component

```ts
public ngOnDestroy() {
  this.componentRef.destroy();
}
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Dynamic Importing 3rd-party Libraries

**Resources**

- ["Angular: Dynamic Importing Large Libraries"](https://medium.com/lacolaco-blog/angular-dynamic-importing-large-libraries-8ec079603d0/)

#### Use import()
`import()` is a new feature of `ECMAScript`. It loads a script dynamically in runtime. In the future, all modern browsers support it natively. But today, its support is not enough.

### Preparation: Edit tsconfig.json

Also, `TypeScript` has support for dynamic `import()`, but it is enabled only in some module types
```json
{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "module": "esnext",
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "importHelpers": true,
    "target": "es5",
    "typeRoots": [
      "node_modules/@types"
    ],
    "lib": [
      "es2018",
      "dom"
    ]
  }
}
```

#### Migrate to dynamic import()

Call import() in the TypeScript code simply like following:
```ts
const importChart = normalizeCommonJSImport(import(/* webpackChunkName: "chart" */ "chart.js"));
```

`normalizeCommonJSImport` is a utility function for compatibility between `CommonJS` module and `ES modules` and for strict-typing.

```ts
export function normalizeCommonJSImport<T>(importPromise: Promise<T>): Promise<T> {
  // CommonJS's `module.exports` is wrapped as `default` in ESModule.
  return importPromise.then((m: any) => (m.default || m) as T);
}
```

In this case, TypeScript’s `import()` returns `Promise<typeof Chart>` as well as `import * as Chart from ‘chart.js’`. This is a problem because `chart.js` is a `CommonJS` module. Without any helpers,default doesn’t exist in the result of `import()` . So we have to mark it as any temporary and remark default as the original type. This is a small hack for correct typing.

As the result, you can see separated bundles like below. `chart.<hash>.js` is not marked as `[initial]`; it means this bundle is loaded lazily and doesn’t affect initial bootstrapping.

<img src="./assets/1_FY_MPUG_xp4BXeWVfZaTXg.png" width="100%" />


<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Unsubscribe from Observables

### Use Async | Pipe

The async pipe subscribes to an Observable or Promise and returns the latest value it has emitted. When a new value is emitted, the async pipe marks the component to be checked for changes. When the component gets destroyed, the asyncpipe unsubscribes automatically to avoid potential memory leaks.
Using it in our AppComponent:

```ts
@Component({
    ...,
    template: `
        <div>
         Interval: {{observable$ | async}}
        </div>
    `
})
export class AppComponent implements OnInit {
    observable$
    ngOnInit () {
        this.observable$ = Rx.Observable.interval(1000);
    }
}
```

On instantiation, the AppComponent will create an Observable from the interval method. In the template, the Observable observable$ is piped to the async Pipe. The async pipe will subscribe to the observable$ and display its value in the DOM. async pipe will unsubscribe the observable\$ when the AppComponent is destroyed. async Pipe has ngOnDestroy on its class so it is called when the view is contained in is being destroyed.
Using the async pipe is a huge advantage if we are using Observables in our components because it will subscribe to them and unsubscribe from them. We will not be bothered about forgetting to unsubscribe from them in ngOnDestroy when the component is being killed off.

### Unsubscribing Declaratively with takeUntil

The solution is to compose our subscriptions with the takeUntil operator and use a subject that emits a truthy value in the ngOnDestroy lifecycle hook.

The following snippet does the exact same thing, but this time we unsubscribe declaratively. You’ll notice that an added benefit is that we don’t need to keep references to our subscriptions anymore:

```ts

import { Observable } from 'rxjs/Observable';
import { Subject } from 'rxjs/Subject';
import 'rxjs/add/observable/interval';
import 'rxjs/add/operator/takeUntil';

@Component({ ... })
export class AppComponent implements OnInit, OnDestroy {
  destroy$: Subject<boolean> = new Subject<boolean>();

  constructor() {}

  ngOnInit(){
    Observable
      .interval(250)
      .takeUntil(this.destroy$)
      .subscribe(val => {
        console.log('Current value:', val);
      });
  }

  ngOnDestroy() {
    this.destroy$.next(true);
    // Now let's also unsubscribe from the subject itself:
    this.destroy$.unsubscribe();
  }
```

Note that Using an operator like takeUntil instead of manually unsubscribing will also complete the observable, triggering any completion event on the observable

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">


## Containerizing Angular using Docker

`Docker` is a popular virtualization tool that replicates a specific operating environment on top of a host OS. Each environment is called a container

A `container` uses an image of a preconfigured operating system optimized for a specific task. When a Docker image is launched, it exists in a container. For example, multiple containers may run the same image at the same time on a single host operating system

**Resources**

- ["Build and run Angular application in a Docker container"](https://wkrzywiec.medium.com/build-and-run-angular-application-in-a-docker-container-b65dbbc50be8/)
- ["Containerizing Angular application for production using Docker"](https://dev.to/usmslm102/containerizing-angular-application-for-production-using-docker-3mhi/)
- [" To List / Start / Stop Docker Containers"](https://phoenixnap.com/kb/how-to-list-start-stop-docker-containers/)

**Pre-requisites**
 - ["Install Docker for Desktop"](https://www.docker.com/products/docker-desktop)

### Creating Docker file

Then create a new file called `Dockerfile` that will be located in the project’s `root` folder. It should have these following lines:

#### Complete Dockerfile

```dockerfile
### STAGE 1: Build ###
FROM node:12.20-alpine3.10 AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build:prod

### STAGE 2: Run ###
FROM nginx:1.17.1-alpine
COPY --from=build /app/dist/ngx-levi9 /usr/share/nginx/html
```

#### STAGE 1: Build

```ts
FROM node:12.20-alpine3.10 As build
```
This line will tell the docker to pull the node image with tag `12.20-alpine3.10` if the images don't exist. We are also giving a friendly name `build` to this image so we can refer it later.

```ts
WORKDIR /app
```
This `WORKDIR` command will create the working directory in our docker image. going forward any command will be run in the context of this directory.

```ts
COPY package.json package-lock.json ./
```
This `COPY` command will copy `package.json` and `package-lock.json` from our current directory to the root of our working directory inside a container which is `/app`.

```ts
RUN npm install
```
This RUN command will restore node_modules define in our package.json

```ts
COPY . .
```
This `COPY` command copies all the files from our current directory to the container working directory. this will copy all our source files

```ts
RUN npm run build:prod
```
This command will build our angular project in production mode and create production ready files in dist/appName folder

#### STAGE 2: Run

```ts
FROM nginx:1.17.1-alpine
```
This line will create a second stage `nginx` container where we will copy the compiled output from our build stage

```ts
COPY --from=build /app/dist/ngx-levi9 /usr/share/nginx/html
```

This is the final command of our docker file. This will copy the compiled angular app from builder stage path `/app/dist/ngx-levi9` to `nginx` container

### Creating docker ignore file
When `COPY . .` command execute it will copy all the files in the host directory to the container working directory. if we want to ignore some folder like `.git` or `node_modules` we can define these folders in `.dockerignore` file
  ```ts
  .git
  node_modules
  ```

### Building a docker image
Navigate the project folder in command prompt and run the below command to build the image

```npm
docker build . -t dockerngstart .
```
This command will look for a docker file in the current directory and create the image with tag `dockerngstart`. with `-t` command you can specify the tag for the image the default convention is `<DockerHubUsername>/<ImageName>`.

### Running a container

You can run the docker image using the below command

```npm
docker run --name dockerngstart-container -it -p 8000:80 dockerngstart
```

A container may be running, but you may not be able to interact with it. To start the container in interactive mode, use the `–i` and `–t` options

Navigate to your browser with `http://localhost:8000`

### Review docker images
Run `docker images` command to list all the docker images in your machine

```ts
docker images
```
<img src="./assets/docker images.png" width="100%" />

### Remove docker image

```ts
docker rmi <IMAGE ID>
```

### List Docker Containers
To list all running Docker containers, enter the following into a terminal window

```ts
docker ps
```
To list all containers, both running and stopped, add `–a` 

```ts
docker ps -a
```

### Start Docker Container
The main command to launch or start a single or multiple stopped Docker containers is docker start:

```ts
docker start [options] container_id 
```

To create a new container from an image and start it, use `docker run`

```ts
docker run [options] image [command] [argument] 
```

### Stop Docker Container

By default, you get a 10 second grace period. The stop command instructs the container to stop services after that period. Use the --time option to define a different grace period expressed in seconds

```ts
docker stop [option] container_id

docker stop --time=20 container_id
```

To immediately kill a docker container without waiting for the grace period to end use:

```ts
docker kill [option] container_id
```

To stop all running containers, enter the following:
```ts
docker stop $(docker ps –a –q)
```

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Angular Schematics

**Resources**

- ["Authoring schematics"](https://angular.io/guide/schematics-authoring/)

- ["Use Angular Schematics to Simplify Your Life"](https://developer.okta.com/blog/2019/02/13/angular-schematics/)

- ["Extend Angular Schematics to customize your development process"](https://indepth.dev/posts/1438/extend-angular-schematics-to-customize-your-development-process/)

A schematic is a template-based code generator that supports complex logic. It is a set of instructions for transforming a software project by generating or modifying code. Schematics are packaged into collections and installed with npm.

The schematic collection can be a powerful tool for creating, modifying, and maintaining any software project, but is particularly useful for customizing Angular projects to suit the particular needs of your own organization. You might use schematics, for example, to generate commonly-used UI patterns or specific components, using predefined templates or layouts. Use schematics to enforce architectural rules and conventions, making your projects consistent and inter-operative

Here is a short list of steps to implement a custom schematic that overrides the default angular one:

- create a blank schematic using the built-in command
- implement schematic
  -  factory function
  -  template file
  -  schema with input parameters definitions
  -  collection, which defines exposed schematics
- add schematic to Angular project

### Schematics CLI

Schematics come with their own command-line tool. Using Node 6.9 or later, install the Schematics command line tool globally:

```npm
$ npm install -g @angular-devkit/schematics-cli
```

Then run schematics to create a new blank project:

```npm
$ schematics blank --name=my-component
```

The blank schematic command will create a project with configured typescript, package.json, and initial schematic. The structure of the project should look like this

```npm
CREATE my-component/README.md (639 bytes)
CREATE my-component/.gitignore (191 bytes)
CREATE my-component/.npmignore (64 bytes)
CREATE my-component/package.json (569 bytes)
CREATE my-component/tsconfig.json (656 bytes)
CREATE my-component/src/collection.json (231 bytes)
CREATE my-component/src/my-component/index.ts (318 bytes)
CREATE my-component/src/my-component/index_spec.ts (503 bytes)
```

`collections.json` - this is the main file, in which are defined all schematics that this project will expose

```json
{
  "$schema": "../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "my-component": {
      "description": "A blank schematic.",
      "factory": "./my-component/index#newModuleSchematics",
      "schema": "./my-component/schema.json"
    }
  }
}
```

You can see that the my-component schematic points to a factory function in `my-component/index.ts`. Crack that open and you’ll see the following

```typescript
import { Rule, SchematicContext, Tree } from '@angular-devkit/schematics';

export function myComponent(_options: any): Rule {
  return (tree: Tree, _context: SchematicContext) => {
    return tree;
  };
}
```

One cool thing about Schematics is they don’t perform any direct actions on your filesystem. Instead, you specify what you’d like to do to a Tree. The Tree is a data structure with a set of files that already exist and a staging area (of files that will contain new/updated code). You can see in the code above that nothing is really happening, the test even proves the tree is empty!

Let’s briefly understand the factory function’s critical elements, which we will later use during the implementation.

`_options` - an object that keeps all input data from a caller. We will use it to get additional inputs as well as the name of the component
`Rule` - it’s an object that defines the transformations of the Tree. All we need to know, for now, is that we will need to build that, and this will precisely define files generation with the rules applicable to them.

### Copy and Manipulate Templates

Writing a template is a relatively easy job because it should look the same as well known generated file, except for one difference - all dynamic content, for example, the name of a component, has to be written inside special tags (<%= , %>), to print the value.

Template files are stored inside `/files` directory, and their names should be written using a specific format to allow dynamic naming of the files. For our case, we want to create a typescript file that will be in the form component-name.component.ts, assuming the "component-name" is our name provided as an input. To fulfill that, we need to create a template file with the name: `__name@dasherize__`.component.ts. Double underscore separates the dynamic content from the plain string, and the dasherize is an Angular function that will make a "kebab-case" string from the name. We have to use the same approach for naming a directory, so we need to place a template file within a folder called `__name@dasherize__`.

Example 

```typescript
// __name@dasherize__.component.ts
import { Component, OnInit } from '@angular/core';

@Component({
	selector: 'app-<%= dasherize(name) %>-component',
	templateUrl: './<%= dasherize(name) %>.component.html',
	styleUrls: ['./<%= dasherize(name) %>.component.scss'],
})
export class <%= classify(name) %>Component implements OnInit {
	
    constructor() { }
	
    ngOnInit(): void {
    }
}
```

### We are running a schematic!

First things first, we need to build our library with a schematic. Use the command below in the library directory

```npm
npm run build
```

Run the following command from the my-component directory.

```npm
schematics .:my-component --dry-run=false
```

This looks like it creates a file, but it does not. This is because schematics runs in debug mode by default. You can bypass by adding `--dry-run=false` to the command. Run `schematics .:my-component --dry-run=false`. If you try running the command again, it’ll fail because the file already exists.

```npm
schematics .:my-component --dry-run=false
An error occured:
Error: Path "/hello.ts" already exist.
```

When using `Schematics`, it’s unlikely you’re going to want to create files and their contents manually. More than likely, you’ll want to copy templates, manipulate their contents, and put them in the project you’re modifying. Luckily, there’s an API for that!

<img src="https://miro.medium.com/max/700/0*Piks8Tu6xUYpF4DU" width="100%" height="17px" style="padding: 2px 1rem; background-color: #fff">

## Performance

**Resources**

- ["Performance Analysis with webpack Bundle Analyzer"](https://www.digitalocean.com/community/tutorials/angular-angular-webpack-bundle-analyzer/)

Web performance is possibly one of the most important parts to take into account for a modern web application. The thing is, it’s easier than ever to add third party modules and tools to our projects, but this can come with a huge performance tradeoff.

This becomes even more difficult the larger a project becomes, therefore, this article looks at how to use webpack Bundle Analyzer with Angular to help visualize where code in the final bundle comes from.

### Webpack Bundle Analyzer

Let’s install the webpack-bundle-analyzer plugin:

```npm
$ npm i webpack-bundle-analyzer -D
```

Building with stats.json:

The Angular CLI gives us the ability to build with a `stats.json` out of the box. This allows us to pass this to our bundle analyzer and start the process.

We can add a new script to `package.json` to add this functionality:

```ts
"scripts": {
  "build:stats": "ng build --stats-json",
  "analyze": "webpack-bundle-analyzer dist/AngularBundleAnalyser/stats.json"
}
```

Now we can run npm run build:stats to generate a stats.json file inside of the dist folder! Let’s do that:

```ts
$ npm run build:stats
```

Run the analyzer with the following command:

```ts
$ npm run analyze
```

Result:
<img src="./assets/webpack-bundle-analysis-2.png" width="960px" />
