Since PhantomJS is using WebKit, a real layout and rendering engine, it can capture a web page as a screenshot.

The following script demonstrates the simplest use of page capture. It loads Google homepage and then save it as an image, `google.png`.

```
var page = require('webpage').create();
page.open('http://google.com', function () {
    page.render('google.png');
    phantom.exit();
});
```

It is possible to build a web screenshot service using PhantomJS. There are [[some projects|Related Projects]] which make it easy to create such a service. Examples of PhantomJS-based screenshot services are [Screener](http://screener.brachium-system.net) and [ChromaNope](http://chromanope.com/).