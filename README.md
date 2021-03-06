# Helpful Koa Routing

A functional routing library for [koa](http://koajs.com/).  Heavily inspired by [koa-route](https://www.npmjs.com/package/koa-route), but with a few adjustments.  This version of the library is meant to be used with Koa v2 and supports promises over generators.

> **Note:** Many of the examples in this documentation use ES2015 destructuring.  ES2015 is _not_ required to use this library!

## Getting Started

Install the package via `npm`:

```
npm install --save helpful-koa-routing
```

Then require it like so...

```js
const route = require('helpful-koa-routing');
```

## Usage

This library exposes functions that make your existing middleware only response to certain HTTP method + path combinations.  The basic methods are supported along with a `route` function for creating routes that support multiple methods.

* `GET()` for `GET` and `HEAD` requests.
* `POST()` for `POST` requests.
* `PUT()` for `PUT` and `PATCH` requests.
* `DELETE()` for `DELETE` requests.

```js
const { route, GET } = require("helpful-koa-routing");

app.use(GET("/", middleware));
app.use(route(["POST", "PUT"], "/", middleware));
```

### Route Parameters

Path parameters, similar to [Express](http://expressjs.com/), can also be used.  These will be made available on your context as a `params` object.

```js
GET("/greet/:name", function (ctx) {
  ctx.body = `Hello,  ${ctx.params.name}`;
});
```

### Multiple Middleware

Typically, it's encouraged to use [koa-compose]() to combine multiple middleware into one before passing it to higher-order functions in order to maintain functional composition and separation of concerns.  However, we've decided to implement middleware composition for you to save the additional step and `require` statement.

To use multiple middleware, simply provide an array of middleware instead of a single middleware function.

```js
GET("/", [
  function (ctx, next) {
    return next();
  },
  function (ctx) {
    ctx.body = 'middleware 2 fired';
  }
]);
```