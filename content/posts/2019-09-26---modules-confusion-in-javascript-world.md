---
title: Modules confusion in javascript world
date: '2019-09-26T08:08:14.162Z'
template: "post"
draft: false
slug: "/posts/modules-confusion-in-javascript-world/"
category: "NodeJs"
tags:
  - "javascript"
  - "nodeJS"
description: CommonJS vs AMD vs ES Modules
socialImage: "/media/image-2.jpg"
---
`ES Modules` ECMAScript 6 a.k.a., ES6 a.ka., ES2015 (Native JavaScript) offers possibilities for importing and exporting modules compatible with both synchronous and asynchronous modes of operation.

However, ES6 module came in 2015 and before that there was no standard defined for modules in javascript.

So before 2015, we had 2 different specifications.

`CommonJS` and `AMD` are 2 specifications (or formats) on how modules and their dependencies should be declared in javascript applications.

 `AMD` is designed to suit the browser environment because it supports asynchronous loading of module dependencies.

whereas `CommonJS` is better suited for server environment.

### Implementations
* `RequireJS` and `curljs` are two libraries implementations of `AMD` specifications.
* `Node.js` and `RingoJS` are the implementing `CommonJS` specifications.

### CommonJS
* Using the keywords `require` and `exports`. 
* `require` is a function used to import functions from another module. `exports` is an object where any function put into it will get exported.
* These modules are designer for server development and these are synchronous.ie., the files are loaded one by one in order inside the file

```js
//------ payments.js ------
var customerStore = require('store/customer'); // import module

//------ store/customer.js ------
exports = function(){
    return customers.get('store);
}
```
