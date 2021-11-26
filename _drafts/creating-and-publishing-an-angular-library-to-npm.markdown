---
layout: single
title: Creating and Publishing an Angular Library to npm
tags:
- angular
- npm
- github
---

Basing on [https://medium.com/angular-in-depth/the-ultimate-guide-to-set-up-your-angular-library-project-399d95b63500](https://medium.com/angular-in-depth/the-ultimate-guide-to-set-up-your-angular-library-project-399d95b63500)

<!--kg-card-begin: html-->

Basic steps:

- Create project, both library, and example / demo website
- Write code for the library
- Write code for the example / demo website
- Setup git hub actions 
  - Compile
  - Publish to NPM
<!--kg-card-end: html-->

Create angular project without creating application

    ng new ng-dice-roller --create-application=false

Create library

    cd ng-dice-roller
    ng generate library ng-dice-roller

Create example app

    ng generate application ng-dice-roller-example

Populate the library with code, service, models, components, etc.

Set the default app in angular.json to the example

Put some examples together

The start doing the config to publish

Steps push, run tests, build and publish the example site, build and publish the library to npm.

Code coverage reporting [https://codecov.io/#features](https://codecov.io/#features)

Add a command to the package.json to run the tests and generate code coverage report

    "test:lib-coverage": "ng test ng-dice-roller --code-coverage --watch=false"

Setup package.json commands for building the examples

    "build": "npm run build:lib && npm run build:website",
    "build:lib": "ng build ng-dice-roller && npm run copy:readme",
    "build:website": "ng build ng-dice-roller-example --base-href='https://bartokw.github.io/ng-dice-roller/'",

And for publishing the example site

    "publish:demo": "npx angular-cli-ghpages --dir=./dist/ng-dice-roller-example"

This deploy step uses `npx` to download and execute the `angular-cli-ghpages`. The `angular-cli-ghpages` then takes care of uploading the passed in directory which is our showcase.

github actions info:

<figure class="kg-card kg-bookmark-card kg-card-hascaption"><a class="kg-bookmark-container" href="https://github.com/actions/starter-workflows"><div class="kg-bookmark-content">
<div class="kg-bookmark-title">actions/starter-workflows</div>
<div class="kg-bookmark-description">Accelerating new GitHub Actions workflows . Contribute to actions/starter-workflows development by creating an account on GitHub.</div>
<div class="kg-bookmark-metadata">
<img class="kg-bookmark-icon" src="https://github.githubassets.com/favicons/favicon.svg"><span class="kg-bookmark-author">GitHub</span><span class="kg-bookmark-publisher">actions</span>
</div>
</div>
<div class="kg-bookmark-thumbnail"><img src="https://avatars2.githubusercontent.com/u/44036562?s=400&amp;v=4"></div></a><figcaption>sample repo</figcaption></figure>

<figure class="kg-card kg-bookmark-card kg-card-hascaption"><a class="kg-bookmark-container" href="https://github.com/actions/starter-workflows/blob/main/ci/node.js.yml"><div class="kg-bookmark-content">
<div class="kg-bookmark-title">actions/starter-workflows</div>
<div class="kg-bookmark-description">Accelerating new GitHub Actions workflows . Contribute to actions/starter-workflows development by creating an account on GitHub.</div>
<div class="kg-bookmark-metadata">
<img class="kg-bookmark-icon" src="https://github.githubassets.com/favicons/favicon.svg"><span class="kg-bookmark-author">GitHub</span><span class="kg-bookmark-publisher">actions</span>
</div>
</div>
<div class="kg-bookmark-thumbnail"><img src="https://avatars2.githubusercontent.com/u/44036562?s=400&amp;v=4"></div></a><figcaption>node</figcaption></figure><figure class="kg-card kg-bookmark-card kg-card-hascaption"><a class="kg-bookmark-container" href="https://github.com/actions/starter-workflows/blob/main/ci/npm-publish.yml"><div class="kg-bookmark-content">
<div class="kg-bookmark-title">actions/starter-workflows</div>
<div class="kg-bookmark-description">Accelerating new GitHub Actions workflows . Contribute to actions/starter-workflows development by creating an account on GitHub.</div>
<div class="kg-bookmark-metadata">
<img class="kg-bookmark-icon" src="https://github.githubassets.com/favicons/favicon.svg"><span class="kg-bookmark-author">GitHub</span><span class="kg-bookmark-publisher">actions</span>
</div>
</div>
<div class="kg-bookmark-thumbnail"><img src="https://avatars2.githubusercontent.com/u/44036562?s=400&amp;v=4"></div></a><figcaption>npm publish</figcaption></figure>