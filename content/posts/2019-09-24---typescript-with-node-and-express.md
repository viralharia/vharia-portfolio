---
title: Typescript with Node and Express
date: '2019-09-24T05:04:45.318Z'
template: "post"
draft: false
slug: "/posts/typescript-with-node-and-express/"
category: "NodeJs"
tags:
  - "Typescript"
  - "nodeJS"
description: 
socialImage: "/media/image-2.jpg"
---
To install `typescript` package globally, use
```javascript
npm install -g typescript
```
OR 
To use only in one project, add typescript in "devDependencies" section of package.json file.
```javascript
npm install typescript --save-dev
```

```javascript
  "devDependencies": {    
    "typescript": "^3.3.4000"
  },
  "dependencies": {
    "express": "^4.16.4"
  }
```
This will give our terminal access to the TypeScript compiler, simply with using the `tsc` command.

## About the Typescript node package
Node.js is an engine that runs Javascript and not Typescript. The node Typescript package allows you to transpile your `.ts` files to `.js` scripts. Babel can also be used to transpile Typescript, however the market standard is to use the official Microsoft package.

Inside our `package.json` we need to put a script called `tsc`:
```javascript
"scripts": {
    "tsc": "tsc"
},
```

This modification allows us to call typescript functions from the command line in the project’s folder. So we can use the following command:
```javascript
npm run tsc -- --init
```
This command initializes the typescript project by creating the `tsconfig.json` file.
`tsconfig.json` files allows us to configure various compiler options for a project such as path where `.js` files should be placed, ECMAScript version we want to compile to etc.

### Installing corresponding Typescript modules of other node modules used in the project
Typescript does not know types of various classes of different node modules used in your project such as express, chai, mocha etc.
So there are specific packages for Typescript to recognize these classes, such as - `@types/express` for express node package.

```javascript
"devDependencies": {
    "@types/bluebird": "^3.5.26",
    "@types/chai": "^4.1.7",
    "@types/express": "^4.16.1",
    "@types/mocha": "^5.2.6",
    "@types/node": "^11.11.6",
    "chai": "^4.2.0",
    "js-yaml": "^3.13.0",
    "mocha": "^6.0.2",
    "ts-node": "^8.0.3",
    "ts-node-dev": "^1.0.0-pre.32",
    "tslint": "^5.14.0",
    "typescript": "^3.3.4000"
  },
  "dependencies": {
    "express": "^4.16.4"
  }
```

### Compiling our application
```javascript
npm run tsc
```

### Running TypeScript without transpiling
You can run typescript directly on the node with the `ts-node` package.

Inside our `package.json`, add 1 more script:
```javascript
"scripts": {
    "dev": "ts-node-dev --respawn --transpileOnly ./app/app.ts"
},
```
and then you can start the development env usnig cmd:
```javascript
npm run dev
```

> Please note, all these dependencies should be placed in "devDependencies" section of package.json file.
And in production env, always use the javascript version of the project in deployment.

### Installing TSLint - Typescript linting library
Add `tslint` in the "devDependencies" section and then run following command to generate - `tslint.json` file
```javascript
tslint --init
```
Use the generated `tslint.json` file to configure various linting parameters.
