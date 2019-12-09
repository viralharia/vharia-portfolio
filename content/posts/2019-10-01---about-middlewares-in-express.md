---
title: About 'Middlewares' in Express
date: '2019-10-01T08:16:51.650Z'
template: "post"
draft: false
slug: "/posts/about-middlewares-in-express/"
category: "NodeJs"
tags:
  - "nodeJS"
  - "express"
description:
socialImage: "/media/image-2.jpg"
---
* Middleware functions are functions that have access to the `request object` (req), the `response object` (res), and the `next` function in the application’s request-response cycle. 
* Middleware functions can perform the following tasks:

  * Execute any code.
  * Make changes to the request and the response objects.
  * End the request-response cycle.
  * Call the next middleware in the stack.
* If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.
* An Express application can use the following types of middleware:

  * Application-level middleware

  * Router-level middleware

  * Error-handling middleware

  * Built-in middleware

  * Third-party middleware

### Application-level middleware
* Bind application-level middleware to an instance of the `app object` by using the `app.use()` and `app.METHOD()` functions, where METHOD is the HTTP method of the request that the middleware function handles (such as GET, PUT, or POST) in lowercase.

Example: middleware function with no mount path. The function is executed every time the app receives a request.

```js
var app = express()

app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

* Example: example of loading a series of middleware functions at a mount point, with a mount path.
```js
app.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

**You can load application-level and router-level middleware with an optional mount path.**

* Example: middleware function mounted on the /user/:id path. The function is executed for any type of HTTP request on the /user/:id path.
```js
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

* Middleware can also be declared in an array for reusability.

* Example: shows an array with a middleware sub-stack that handles GET requests to the /user/:id path

```js
function logOriginalUrl (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}

function logMethod (req, res, next) {
  console.log('Request Type:', req.method)
  next()
}

var logStuff = [logOriginalUrl, logMethod]
app.get('/user/:id', logStuff, function (req, res, next) {
  res.send('User Info')
})
```

### Router level middleware
* Router-level middleware works in the same way as application-level middleware, except it is bound to an instance of `express.Router()`.
* Load router-level middleware by using the `router.use()` and `router.METHOD()` functions.

### Error-handling middleware
* Define error-handling middleware functions in the same way as other middleware functions, except with four arguments instead of three, specifically with the signature `(err, req, res, next))`:
```js
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```


** The order of middleware loading is important: middleware functions that are loaded first are also executed first.
**
