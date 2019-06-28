# Writing an API

The [API module](https://github.com/taylortom/adapt-authoring-api) makes defining custom APIs simple by providing abstract classes and utilities you can use in your own modules to replace the common boilerplate code required when writing an API.

## Defining your API

At the very least, each new Api subclass must override the static **def** variable. This is then used by the [Api base-class](../class/adapt_authoring_restructure/adapt-authoring-api/lib/module.js~Api.html) to set up the API router, as well as any middleware and route handlers. Here are a few tips:

- Use the API's middleware to perform any tasks which are common to all routes, such as checking and formatting the request params
- You only need to specify route handlers for the routes/HTTP methods you want to enable, access will be blocked to any route/HTTP method combinations you haven't defined
- You can run route-specific middleware by adding it as a handler to the route config (see example 2. below)

<br>For full details on what should be returned by _def_, see the [Api#def reference](../class/adapt_authoring_restructure/adapt-authoring-api/lib/module.js~Api.html#static-get-def).



Some example configurations:
```javascript
/*
* 1. Basic configuration
*/
{
  name: 'helloworld',
  routes: [{
    route: '/',
    handlers: { get: (req, res, next) => { res.send('Hello world!') } }
  }]
}
/*
* 2. Full configuration
*/
{
  name: 'helloworld',
  schemas: [ HelloWorldModel ],
  middleware: [ middlewareFunction ],
  routes: [
    {
      route: '/:id?',
      handlers: {
        post: [beforePost, postFunction, afterPost],
        get: getFunction,
        put: putFunction,
        delete: deleteFunction
      }
    }
  ]
}
```