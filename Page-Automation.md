Because PhantomJS can load and manipulate a web page, it is perfect to carry out various page automations.

## DOM Manipulation

Since the script is executed as if it is running on a web browser, standard [DOM scripting](http://en.wikipedia.org/wiki/DOM_scripting) and [CSS selectors](http://www.w3.org/TR/css3-selectors/) work just fine.

The following `useragent.js` example demonstrates reading the `innerText` property of the element whose *id* is `myagent`:

```javascript
var page = require('webpage').create();
console.log('The default user agent is ' + page.settings.userAgent);
page.settings.userAgent = 'SpecialAgent';
page.open('http://www.httpuseragent.org', function (status) {
    if (status !== 'success') {
        console.log('Unable to access network');
    } else {
        var ua = page.evaluate(function () {
            return document.getElementById('myagent').innerText;
        });
        console.log(ua);
    }
    phantom.exit();
});
```
The above example also demonstrates a way to customize the user agent seen by the remote web server.