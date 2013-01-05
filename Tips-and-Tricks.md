### Page background color

PhantomJS does not set the background color of the web page at all, it is left to the page to decide its background color. If the page does not set anything, then it remains transparent.

To force a certain color for the web page background, use the following code:

```javascript
page.evaluate(function() {
    document.body.bgColor = 'white';
});
```
Make sure this is executed before caling render() function of the web page.

### Access to standard stream

This is possible using the Filesystem API.

An example to write directly to standard output:

```javascript
var fs = require("fs"); 
fs.write("/dev/stdout", "Hello, world!", "w");
```

Caveat: it may not work on Windows.