Since PhantomJS is using WebKit, a real layout and rendering engine, it can capture a web page as a screenshot. Because PhantomJS can render anything on the web page, it can be used to convert contents not only in HTML and CSS, but also SVG and Canvas.

The following script demonstrates the simplest use of page capture. It loads Google homepage and then save it as an image, `google.png`.

```
var page = require('webpage').create();
page.open('http://google.com', function () {
    page.render('google.png');
    phantom.exit();
});
```

Beside PNG format, PhantomJS supports JPEG, GIF, and PDF.

In the `examples` subdirectory, there is a script `rasterize.js` which demonstrates a more complete rendering feature of PhantomJS. An example to produce the rendering of the famous Tiger (from SVG):

```
phantomjs rasterize.js http://ariya.github.com/svg/tiger.svg tiger.png
```
which gives the following tiger.png:

![Rendered Tiger](http://lh6.ggpht.com/_Oijhf1ZPv-4/TR6iM8J0KrI/AAAAAAAABy4/RCZ8Eg567LM/s400/tiger.png)

It is possible to build a web screenshot service using PhantomJS. There are [[some projects|Related Projects]] which make it easy to create such a service. Examples of PhantomJS-based screenshot services are [Screener](http://screener.brachium-system.net) and [ChromaNope](http://chromanope.com/).