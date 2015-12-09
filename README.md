# express-mung [![Build Status](https://travis-ci.org/richardschneider/express-prefer.svg)](https://travis-ci.org/richardschneider/express-mung)

Middleware for express responses.

This package allows synchronous and asynchronous transformation of an express response.  This is a similar concept to the express middleware for a request but for a response.  Note that the middleware is executed in LIFO order.  It is implemented by monkey patching (hooking) the `res.end` or `res.json` methods.


## Getting started [![npm version](https://badge.fury.io/js/express-mung.svg)](https://badge.fury.io/js/express-mung)

    $ npm install express-mung --save

Then in your middleware

    var mung = require('express-mung');

    module.exports = mung.json(my_transform);

## Usage

Sample middleware (redact.js) to remove classified information.

````javascript
'use strict';
const mung = require('express-mung');

/* Remove any classified information from the response. */
function redact(body, req, res) {
    // ...
    return body;
}

exports = mung.json(redact);
````

then add to your `server.js` file
````javascript
app.use(require('redact.js'))
````
and [*That's all folks!*](https://www.youtube.com/watch?v=gBzJGckMYO4)

## Reference

- `mung.json(fn)` transform the JSON body of the response.  `fn` receives the JSON as an object, the `req` and `res`.  It returns the modified body. If `undefined` is returned (i.e. nothing) then original JSON is assumed to be modified.  If `null` is returned, then a 204 No Content HTTP status is returned to client.

- `mung.jsonAsync(fn)` transform the JSON body of the response.  `fn` receives the JSON as an object, the `req` and `res`.  It returns a promise to a modified body.  The promise returns an object.  If it is `null` then a 204 No Content is sent to the client.

# License
The MIT license

Copyright © 2015 Richard Schneider (makaretu@gmail.com)