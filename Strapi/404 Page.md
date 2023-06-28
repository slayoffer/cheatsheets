When Strapi receives a request for a non-existent page or resource, it will still return a 200 status code by default. This is because Strapi uses a catch-all route to handle all incoming requests, regardless of whether the requested resource exists or not.

To change this behavior, you can modify the `middlewares.js` file in your Strapi project. Specifically, you can add a custom middleware function to handle 404 errors and return a different status code, such as 404.

Here's an example of how you can modify the `middlewares.js` file to handle 404 errors:

```js
module.exports = {
  // ... other middleware functions ...

  // This function will handle 404 errors
  async 404(ctx, next) {
    ctx.status = 404;
    ctx.body = {
      error: 'Not found'
    };
    await next();
  }
};
```

With this middleware function in place, Strapi will now return a 404 status code and a JSON response with an error message for any requests to non-existent resources.

----

To set a custom 404 page in Strapi, you can follow these steps:

1. Create a new file in the `./public` directory of your Strapi project called `404.html`.

2. Add your custom content to the `404.html` file. This could include a message to the user, links to relevant pages, or any other content you want to display.

3. Edit the `./config/middleware.js` file in your Strapi project. Add the following code at the end of the file:

   ```js
   module.exports = {
     settings: {
       // Other middleware settings here...
       public: {
         maxAge: 60000,
         etag: true,
         lastModified: true,
         // Add the following lines to set the 404 page
         staticOptions: {
           redirect: false,
         },
         serveStatic: {
           index: false,
           // Set the path to your custom 404 page here
           path: "./public/404.html",
         },
       },
     },
   };
   ```

4. Save the `middleware.js` file.

5. Restart your Strapi server.

Now, when a user tries to access a page that doesn't exist on your Strapi site, they will be shown your custom 404 page instead of the default error page.

----

Here's an example of how you can modify the middleware in Strapi to ensure that a 404 status code is always returned for requests to non-existent resources:

```js
// Define a custom middleware function
const customMiddleware = async (ctx, next) => {
  await next(); // Call the next middleware in the chain

  // If the response status is 404 and the response body is empty,
  // set the response status to 404 and return an error message
  if (ctx.status === 404 && !ctx.body) {
    ctx.status = 404;
    ctx.body = { error: 'Resource not found' };
  }
};

// Register the custom middleware function in the Strapi middleware chain
// This should be done in the `./config/middleware.js` file
module.exports = {
  settings: {
    // ...
  },
  load: {
    // ...
  },
  settings: {
    // ...
  },
  // Add the custom middleware function to the middleware chain
  order: [
    // ...
    'customMiddleware',
  ],
};
```

In this example, we define a custom middleware function that checks if the response status is 404 and the response body is empty. If this is the case, we set the response status to 404 and return an error message. We then register this middleware function in the Strapi middleware chain by adding it to the `order` array in the `./config/middleware.js` file.

With this middleware in place, Strapi will always return a 404 status code for requests to non-existent resources, regardless of whether a plugin or middleware intercepts the request.

----

Here's an example of what a `middlewares.js` file might look like in a Strapi project:

```js
module.exports = {
  // This function will log all incoming requests
  async logger(ctx, next) {
    console.log(`[${new Date().toISOString()}] ${ctx.request.method} ${ctx.request.url}`);
    await next();
  },

  // This function will handle 404 errors
  async 404(ctx, next) {
    ctx.status = 404;
    ctx.body = {
      error: 'Not found'
    };
    await next();
  },

  // This function will handle 500 errors
  async errorHandler(ctx, next) {
    try {
      await next();
    } catch (err) {
      console.error(err);
      ctx.status = err.status || 500;
      ctx.body = {
        error: err.message
      };
    }
  },

  // ... other middleware functions ...
};
```

In this example, we have three middleware functions:

- `logger`: This function logs all incoming requests to the console, including the request method and URL.
- `404`: This function handles 404 errors by setting the response status code to 404 and returning a JSON response with an error message.
- `errorHandler`: This function handles all other errors by catching any errors thrown by downstream middleware functions or route handlers, setting the response status code to the error status code or 500 if no status code is provided, and returning a JSON response with an error message.

You can add additional middleware functions to this file as needed to customize the behavior of your Strapi application.