This instruction assumes that PhantomJS is installed and its executable is place somewhere in the PATH.

All of the examples given here are available in the code repository under the sub-directory `examples/`. For each example, there are two version for each JavaScript and CoffeeScript.

## Hello, World!

Create a new text file that contains the following two lines:

```javascript
console.log('Hello, world!');
phantom.exit();
```

Save it as `hello.js` and the run it:

    phantomjs hello.js

The output is:

> Hello, world!

In the first line, `console.log` will print the passed string to the terminal. In the second line, `phantom.exit` terminates the execution.

It is **very important** to call `phantom.exit` at some point in the script, otherwise PhantomJS will not be terminated at all.

## Page Loading

A web page can be loaded, analyzed, and rendered by creating a web page object.

The following script demonstrates the simplest use of page object. It loads Google homepage and then save it as an image, `google.png`.

```
var page = require('webpage').create();
page.open('http://google.com', function () {
    page.render('google.png');
    phantom.exit();
});
```

Because of its rendering features, PhantomJS can be used to [[capture web pages|Screen Capture]], essentially taking a screenshot of the contents.

The following `loadspeed.js` script loads a specified URL (do not forget the http protocol) and measures the time it takes to load it.

```javascript
var page = require('webpage').create(),
    t, address;

if (phantom.args.length === 0) {
    console.log('Usage: loadspeed.js <some URL>');
    phantom.exit();
} else {
    t = Date.now();
    address = phantom.args[0];
    page.open(address, function (status) {
        if (status !== 'success') {
            console.log('FAIL to load the address');
        } else {
            t = Date.now() - t;
            console.log('Loading time ' + t + ' msec');
        }
        phantom.exit();
    });
}
```

Run the script with the command:

    phantomjs loadspeed.js http://www.google.com

It outputs something like:

> Loading http://www.google.com
> Loading time 719 msec

(More to be written)

## Script Arguments

(To be written)

## Page Settings

(To be written)

## Code Evaluation

To evaluate JavaScript or CoffeeScript code in the context of the web page, use `evaluate()` function. The execution is "sandboxed", there is no way for the code to access any JavaScript objects and variables outside its own page context. An object can be returned from `evaluate()`, however it is limited to simple objects and can't contain functions or closures.

Here is an example to show the title of a web page:

```javascript
var page = require('webpage').create();
page.open(url, function (status) {
    var title = page.evaluate(function () {
        return document.title;
    });
    console.log('Page title is ' + title);
});
```

Any console message from a web page, including from the code inside `evaluate()`, will not be displayed by default. To override this behavior, use the `onConsoleMessage` callback. The previous example can be rewritten to:

```javascript
var page = require('webpage').create();
page.onConsoleMessage = function (msg) {
    console.log('Page title is ' + msg);
};
page.open(url, function (status) {
    page.evaluate(function () {
        console.log(document.title);
    });
});
```