_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].


<a name="webserver-module" />
## Module: WebServer ##
**Stability:** _EXPERIMENTAL_  
**Introduced:** PhantomJS 1.4  
Using an embedded web server module called [Mongoose](http://code.google.com/p/mongoose/), PhantomJS script can start a web server. This is intended for ease of communication between PhantomJS scripts and the outside world and is _not_ recommended for use as a general production server.

Here is a simple example that always gives the same response regardless of the request:
```js
var server = require('webserver').create();
var service = server.listen(8080, function(request, response) {
    response.statusCode = 200;
    response.write('<html><body>Hello!</body></html>');
    response.close();
});
```
If you want to bind to specify address, just use `ipaddress:port` instead of `port`.
Example:
```js
var server = require('webserver').create();
var service = server.listen('127.0.0.1:8080', function(request, response) {
    response.statusCode = 200;
    response.write('<html><body>Hello!</body></html>');
    response.close();
});
```
The value returned by `server.listen()` is a boolean: true if the server is launched.

The `server` object contains these functions:
 * `close()` : close the server
 * `port` : the port on which the server listen requests (readonly)


<a name="webserver-request" />
The `request` object passed to the callback function may contain the following properties:
 * `method`: Defines the request method (`'GET'`, `'POST'`, etc.)
 * `url`: The path part and query string part (if any) of the request URL
 * `httpVersion`: The actual HTTP version
 * `headers`: All of the HTTP headers as key-value pairs
 * `post`: The request body (only for `'POST'` and `'PUT'` method requests)
 * `postRaw`: If the `Content-Type` header is set to `'application/x-www-form-urlencoded'` (the default for form submissions), the original contents of `post` will be stored in this extra property (`postRaw`) and then `post` will be automatically updated with a URL-decoded version of the data.
 
<a name="webserver-response" />
The `response` object should be used to create the response using the following properties and functions:
 * `headers`: Stores all HTTP headers as key-value pairs. These must be set **BEFORE** calling `write` for the first time.
 * `setHeader(name, value)`: sets a specific header
 * `header(name)`: returns the value of the given header
 * `statusCode`: Sets the returned HTTP status code.
 * `setEncoding(encoding)`: Indicate the encoding to use to convert data given to `write()`. By default data will be converted to UTF-8. Indicate `"binary"` if data is a binary string.
 * `write(data)`: Sends a chunk for the response body. Can be called multiple times.
 * `writeHead(statusCode, headers)`: Sends a response header to the request. The status code is a 3-digit HTTP status code (e.g. `404`). The last argument, headers, are the response headers. Optionally one can give a human-readable `headers` collection as the second argument.
 * `close()`: Closes the HTTP connection.
      * To avoid the client detecting a connection drop, remember to use `write()` at least once. Sending an empty string (i.e. `response.write('')`) would be enough if the only aim is, for example, to return an HTTP status code of `200` (`"OK"`).
 * `closeGracefully()`: same as `close()`, but ensures response headers have been sent first (and do at least a `response.write('')`)
