_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

<a name="webpage-module" />
## Module: WebPage ##
A `WebPage` object encapsulates a web page. It is usually instantiated using the following pattern:
```js
var page = require('webpage').create();
```

**Note:** For backward compatibility with legacy PhantomJS applications, the constructor also remains exposed as a _deprecated_ global `WebPage` object:
```js
var page = new WebPage();
```

<a name="webpage-properties" />
### Properties ###

<a name="webpage-clipRect" />
#### `clipRect` {object} ####
This property defines the rectangular area of the web page to be rasterized when [`WebPage#render`](#webpage-render) is invoked. If no clipping rectangle is set, [`WebPage#render`](#webpage-render) will process the entire web page.

Example:
```js
page.clipRect = { top: 14, left: 3, width: 400, height: 300 };
```

<a name="webpage-content" />
#### `content` {string} ####
This property stores the content of the web page (main frame), enclosed in an HTML/XML element. Setting the property will effectively reload the web page with the new content.

<a name="webpage-cookies" />
#### `cookies` {[Cookie](#cookie)[]} ####
Get or set [Cookies](#cookie) visible to the current URL (though, for setting, use of [`WebPage#addCookie`](#webpage-addCookie) is preferred). This array will be pre-populated by any existing Cookie data visible to this URL that is stored in the CookieJar, if any. See [`phantom.cookies`](#phantom-cookies) for more information on the CookieJar.

<a name="webpage-customHeaders" />
#### `customHeaders` {object} ####
**Introduced:** PhantomJS 1.5  
This property specifies additional HTTP request headers that will be sent to the server for every request issued (for pages _and_ resources). The default value is an empty object `{}`. Headers names and values get encoded in US-ASCII before being sent. Please note that the 'User-Agent' should be set using the [`WebPage#settings`](#webpage-settings), setting the 'User-Agent' property in this property will _overwrite_ the value set via `WebPage#settings`.

Example:
```js
// Send two additional headers 'X-Test' and 'DNT'.
page.customHeaders = {
    'X-Test': 'foo',
    'DNT': '1'
};
```

Do you only want these `customHeaders` passed to the initial [`WebPage#open`](#webpage-open) request? Here's the recommended workaround:
```js
// Send two additional headers 'X-Test' and 'DNT'.
page.customHeaders = {
    'X-Test': 'foo',
    'DNT': '1'
};
page.onInitialized = function() {
    page.customHeaders = {};
};
```


<a name="webpage-frameContent" />
#### `frameContent` {string} ####
**Introduced:** PhantomJS 1.7  
This property stores the content of the web page's _currently active_ frame (which may or may not be the main frame), enclosed in an HTML/XML element. Setting the property will effectively reload the web page with the new content.

<a name="webpage-framePlainText" />
#### `framePlainText` {string} ####
**Introduced:** PhantomJS 1.7  
Read-only. This property stores the content of the web page's _currently active_ frame (which may or may not be the main frame) as plain text &mdash; no element tags!

<a name="webpage-frameUrl" />
#### `frameUrl` {string} ####
**Introduced:** PhantomJS 1.7  
Read-only. This property gets the current URL of the web page's _currently active_ frame (which may or may not be the main frame).

<a name="webpage-libraryPath" />
#### `libraryPath` {string} ####
This property stores the path which is used by [`WebPage#injectJs`](#webpage-injectJs) function to
resolve the script name. Initially it is set to the location of the
script invoked by PhantomJS.

<a name="webpage-navigationLocked" />
#### `navigationLocked` {boolean} ####
This property defines whether navigation away from the page is permitted or not. If it is set to `true`, then the page is locked to the current URL. Defaults to `false`.

<a name="webpage-paperSize" />
#### `paperSize` {object} ####
This property defines the size of the web page when rendered as a PDF.

The given object should be in one of the following two formats:
```js
{ width: '200px', height: '300px', border: '0px' }
{ format: 'A4', orientation: 'portrait', border: '1cm' }
```

If no `paperSize` is defined, the size is defined by the web page. Supported dimension units are: `'mm'`, `'cm'`, `'in'`, `'px'`. No unit means `'px'`. Border is optional and defaults to `0`. Supported formats are: `'A3'`, `'A4'`, `'A5'`, `'Legal'`, `'Letter'`, `'Tabloid'`. Orientation (`'portrait'`, `'landscape'`) is optional and defaults to `'portrait'`.

Example:
```js
page.paperSize = { width: '5in', height: '7in', border: '20px' };
```

<a name="webpage-plainText" />
#### `plainText` {string} ####
Read-only. This property stores the content of the web page (main frame) as plain text &mdash; no element tags!

<a name="webpage-scrollPosition" />
#### `scrollPosition` {object} ####
This property defines the scroll position of the web page.

Example:
```js
page.scrollPosition = { top: 100, left: 0 };
```

<a name="webpage-settings" />
#### `settings` {object} ####
This property stores various settings of the web page:
 * `javascriptEnabled` defines whether to execute the script in
  the page or not (defaults to `true`).
 * `loadImages` defines whether to load the inlined images or not (defaults to `true`).
 * `localToRemoteUrlAccessEnabled` defines whether local resource (e.g. from file) can access remote URLs or not (defaults to `false`).
 * `userAgent` defines the user agent sent to server when the web page requests resources.
 * `userName` sets the user name used for HTTP authentication.
 * `password` sets the password used for HTTP authentication.
 * `XSSAuditingEnabled` defines whether load requests should be monitored for cross-site scripting attempts (defaults to `false`).
 * `webSecurityEnabled` defines whether web security should be
  enabled or not (defaults to `true`).

**Note:** The `settings` apply only during the initial call to the [`WebPage#open`](#webpage-open) function. Subsequent modification of the `settings` object will not have any impact.

<a name="webpage-url" />
#### `url` {string} ####
**Introduced:** PhantomJS 1.7  
Read-only. This property gets the current URL of the web page (main frame).

<a name="webpage-viewportSize" />
#### `viewportSize` {object} ####
This property sets the size of the viewport for the layout process. It is useful to set the preferred initial size before loading the page, e.g. to choose between `'landscape'` vs `'portrait'`.

Because PhantomJS is headless (nothing is shown), `viewportSize` effectively simulates the size of the window like in a traditional browser.

Example:
```js
page.viewportSize = { width: 480, height: 800 };
```

<a name="webpage-zoomFactor" />
#### `zoomFactor` {number} ####
This property specifies the scaling factor for the [`WebPage#render`](#webpage-render) and [`WebPage#renderBase64`](#webpage-renderBase64) functions. The default is `1`, i.e. 100% zoom.

Example:
```js
// Create a thumbnail preview with 25% zoom
page.zoomFactor = 0.25;
page.render('capture.png');
```



<a name="webpage-functions" />
### Functions ###

<a name="webpage-addCookie" />
#### `addCookie([Cookie](#cookie))` {boolean} ####
**Introduced:** PhantomJS 1.7  
Add a [Cookie](#cookie) to the page.  If the domains do not match, the Cookie will be ignored/rejected. Returns `true` if successfully added, otherwise `false`.

Example:
```js
page.addCookie({
    'name': 'Added-Cookie-Name',
    'value': 'Added-Cookie-Value'
});
```


<a name="webpage-clearCookies" />
#### `clearCookies()` {void} ####
**Introduced:** PhantomJS 1.7  
Delete all [Cookies](#cookie) visible to the current URL.

<a name="webpage-close" />
#### `close()` {void} ####
**Introduced:** PhantomJS 1.7  
Close the page and releases the memory heap associated with it. Do not use the page instance after calling this.

Due to some technical limitations, the web page object might not be completely garbage collected. This is often encountered when the same object is used over and over again. Calling this function may stop the increasing heap allocation.

<a name="webpage-deleteCookie" />
#### `deleteCookie(cookieName)` {boolean} ####
**Introduced:** PhantomJS 1.7  
Delete any [Cookies](#cookie) visible to the current URL with a 'name' property matching `cookieName`. Returns `true` if successfully deleted, otherwise `false`.

Example:
```js
page.deleteCookie('Added-Cookie-Name');
```


<a name="webpage-evaluate" />
#### `evaluate(function, arg1, arg2, ...)` {object} ####
Evaluates the given function in the context of the web page. The execution is sandboxed, the web page has no access to the `phantom` object and it can't probe its own setting.

Example:
```js
var page = require('webpage').create();
page.open('http://m.bing.com', function(status) {
    var title = page.evaluate(function() {
        return document.title;
    });
    console.log(title);
    phantom.exit();
});
```

As of PhantomJS 1.6, JSON-serializable arguments can be passed to the function. In the following example, the text value of a DOM element is extracted. The following example achieves the same end goal as the previous example but the element is chosen based on a selector which is passed to the `evaluate` call:
```js
var page = require('webpage').create();
page.open('http://m.bing.com', function(status) {
    var title = page.evaluate(function(s) {
        return document.querySelector(s).innerText;
    }, 'title');
    console.log(title);
    phantom.exit();
});
```

**Note:** The arguments and the return value to the `evaluate` function must be a simple primitive object. The rule of thumb: if it can be serialized via JSON, then it is fine. Closures,
functions, DOM nodes, etc. will _not_ work!

<a name="webpage-evaluateAsync" />
#### `evaluateAsync(function)` {void} ####
Evaluates the given function in the context of the web page without blocking the current execution. The function returns immediately and there is no return value. This is useful to run some script asynchronously.

<a name="webpage-includeJs" />
#### `includeJs(url, callback)` {void} ####
Includes external script from the specified `url` (usually a remote location) on the page and executes the `callback` upon completion.

Example:
```js
page.includeJs('http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js', function() {
    /* jQuery is loaded, now manipulate the DOM */
});
```

<a name="webpage-injectJs" />
#### `injectJs(filename)` {boolean} ####
Injects external script code from the specified file into the page (like [`WebPage#includeJs`](#webpage-includeJs), except that the file does not need to be accessible from the hosted page). If the file cannot be found in the current directory, `libraryPath` is used for additional look up. This function returns `true` if injection is successful, otherwise it returns `false`.

<a name="webpage-open" />
#### `open(url, callback)` {void} ####
Opens the `url` and loads it to the page. Once the page is loaded, the optional `callback` is called using [`WebPage#onLoadFinished`](#webpage-onLoadFinished), and also provides the page status to the function (`'success'` or `'fail'`).

Example:
```js
page.open('http://www.google.com/', function(status) {
    console.log('Status: ' + status);
    // Do other things here...
});
```

<a name="webpage-release" />
#### `release()` {void} ####
**Stability:** _DEPRECATED_ - Use [`WebPage#close`](#webpage-close))  
Releases memory heap associated with this page. Do not use the page instance after calling this.

Due to some technical limitations, the web page object might not be completely garbage collected. This is often encountered when the same object is used over and over again. Calling this function may stop the increasing heap allocation.

<a name="webpage-render" />
#### `render(filename)` {void} ####
Renders the web page to an image buffer and saves it as the specified `filename`.

Currently, the output format is automatically set based on the file extension. Supported formats include:
 * PNG
 * GIF
 * JPEG
 * PDF

<a name="webpage-renderBase64" />
#### `renderBase64(format)` ####
Renders the web page to an image buffer and returns the result as a [Base64](http://en.wikipedia.org/wiki/Base64)-encoded string representation of that image.

Supported formats include:
 * PNG
 * GIF
 * JPEG

<a name="webpage-sendEvent" />
#### `sendEvent(mouseEventType[, mouseX, mouseY, button='left'])` or `sendEvent(keyboardEventType, keyOrKeys)` ####
Sends an event to the web page. [1.7 implementation source](https://github.com/ariya/phantomjs/blob/63e06cb/src/webpage.cpp#L1015).

The events are not like synthetic [DOM events](http://www.w3.org/TR/DOM-Level-2-Events/events.html). Each event is sent to the web page as if it comes as part of user interaction.

##### Mouse events #####

The first argument is the event type. Supported types are `'mouseup'`, `'mousedown'`, `'mousemove'`, `'doubleclick'` and `'click'`. The next two arguments are optional but represent the mouse position for the event.

The button parameter (defaults to `left`) specifies the button to push.

For `'mousemove'`, however, there is no button pressed (i.e. it is not dragging).

##### Keyboard events #####

The first argument is the event type. The supported types are: `keyup`, `keypress` and `keydown`. The second parameter is a key (from [page.event.key](https://github.com/ariya/phantomjs/commit/cab2635e66d74b7e665c44400b8b20a8f225153a)), or a string.

<a name="webpage-setContent" />
#### `setContent(content, url)` ####
**Introduced:** PhantomJS 1.8

Allows to set both [`WebPage#content`](#webpage-content) and [`WebPage#url`](#webpage-url) properties.

The webpage will be reloaded with the new content and the current location set as the given url, without any actual http request being made.

<a name="webpage-uploadFile" />
#### `uploadFile(selector, filename)` ####
Uploads the specified file (`filename`) to the form element associated with the `selector`.

This function is used to automate the upload of a file, which is usually handled with a file dialog in a traditional browser. Since there is no dialog in this headless mode, such an upload mechanism is handled via this special function instead.

Example:
```js
page.uploadFile('input[name=image]', '/path/to/some/photo.jpg');
```

<a name="webpage-callbacks" />
### Callbacks ###

<a name="webpage-onAlert" />
#### `onAlert` ###
**Introduced:** PhantomJS 1.0  
This callback is invoked when there is a JavaScript `alert` on the web page. The only argument passed to the callback is the string for the message. There is no return value expected from the callback handler.

Example:
```js
page.onAlert = function(msg) {
    console.log('ALERT: ' + msg);
};
```

<a name="webpage-onCallback" />
#### `onCallback` ####
**Stability:** _EXPERIMENTAL_  
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `window.callPhantom` call made on the web page. The only argument passed to the callback is a data object.

**Note**: `window.callPhantom` is still an experimental API. In the near future, it will be likely replaced with a message-based solution which will still provide the same functionality.

Although there are many possible use cases for this inversion of control, the primary one so far is to prevent the need for a PhantomJS script to be continually polling for some variable on the web page.

Example:  
_WebPage (client-side)_  
```js
if (typeof window.callPhantom === 'function') {
    window.callPhantom({ hello: 'world' });
}
```

_PhantomJS (server-side)_  
```js
page.onCallback = function(data) {
    console.log('CALLBACK: ' + JSON.stringify(data));  // Prints 'CALLBACK: { "hello": "world" }'
};
```

Additionally, note that the `WebPage#onCallback` handler can return a data object that will be curried back as the result of the originating `window.callPhantom` call, too.

Example:  
_WebPage (client-side)_  
```js
if (typeof window.callPhantom === 'function') {
    var status = window.callPhantom({ secret: 'ghostly' });
    alert(status);  // Will either print 'Accepted.' or 'DENIED!'
}
```

_PhantomJS (server-side)_  
```js
page.onCallback = function(data) {
    if (data && data.secret && (data.secret === 'ghostly') {
        return 'Accepted.';
    }
    return 'DENIED!';
};
```


<a name="webpage-onClosing" />
#### `onClosing` ####
**Introduced:** PhantomJS 1.7  
This callback is invoked when the `WebPage` object is being closed, either via [`WebPage#close`](#webpage-close) in the PhantomJS outer space or via [`window.close`](https://developer.mozilla.org/docs/DOM/window.close) in the page's client-side.  It is _not_ invoked when child/descendant pages are being closed unless you also hook them up individually. It takes one argument, `closingPage`, which is a reference to the page that is closing. Once the `onClosing` handler has finished executing (returned), the `WebPage` object `closingPage` will become invalid.

Example:
```js
page.onClosing = function(closingPage) {
    console.log('The page is closing! URL: ' + closingPage.url);
};
```

<a name="webpage-onConfirm" />
#### `onConfirm` ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `confirm` on the web page. The only argument passed to the callback is the string for the message. The return value of the callback handler can be either `true` or `false`, which are equivalent to pressing the "OK" or "Cancel" buttons presented in a JavaScript `confirm`, respectively.

Example:
```js
page.onConfirm = function(msg) {
    console.log('CONFIRM: ' + msg);
    return true;  // `true` === pressing the "OK" button, `false` === pressing the "Cancel" button
};
```

<a name="webpage-onConsoleMessage" />
#### `onConsoleMessage` ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when there is a JavaScript `console` message on the web page. The callback may accept up to three arguments: the string for the message, the line number, and the source identifier.

By default, `console` messages from the web page are not displayed. Using this callback is a typical way to redirect it.

Example:
```js
page.onConsoleMessage = function(msg, lineNum, sourceId) {
    console.log('CONSOLE: ' + msg + ' (from line #' + lineNum + ' in "' + sourceId + '")');
};
```

<a name="webpage-onError" />
#### `onError` ####
**Introduced:** PhantomJS 1.5  
This callback is invoked when there is a JavaScript execution error. It is a good way to catch problems when evaluating a script in the web page context. The arguments passed to the callback are the error message and the stack trace [as an Array].

Example:
```js
page.onError = function(msg, trace) {
    var msgStack = ['ERROR: ' + msg];
    if (trace) {
        msgStack.push('TRACE:');
        trace.forEach(function(t) {
            msgStack.push(' -> ' + t.file + ': ' + t.line + (t.function ? ' (in function "' + t.function + '")' : ''));
        });
    }
    console.error(msgStack.join('\n'));
};
```

<a name="webpage-onInitialized" />
#### `onInitialized` ####
**Introduced:** PhantomJS 1.3  
This callback is invoked _after_ the web page is created but _before_ a URL is loaded. The callback may be used to change global objects.

Example:
```js
page.onInitialized = function() {
    page.evaluate(function() {
        document.addEventListener('DOMContentLoaded', function() {
            console.log('DOM content has loaded.');
        }, false);
    });
};
```

<a name="webpage-onLoadFinished" />
#### `onLoadFinished` ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page finishes the loading. It may accept a single argument indicating the page's `status`: `'success'` if no network errors occurred, otherwise `'fail'`.

Also see [`WebPage#open`](#webpage-open) for an alternate hook for the `onLoadFinished` callback.

Example:
```js
page.onLoadFinished = function(status) {
    console.log('Status: ' + status);
    // Do other things here...
};
```

<a name="webpage-onLoadStarted" />
#### `onLoadStarted` ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page starts the loading. There is no argument passed to the callback.

Example:
```js
page.onLoadStarted = function() {
    var currentUrl = page.evaluate(function() {
        return window.location.href;
    });
    console.log('Current page ' + currentUrl +' will gone...');
    console.log('Now loading a new page...');
};
```

<a name="webpage-onNavigationRequested" />
#### `onNavigationRequested` ####
**Introduced:** PhantomJS 1.6  
By implementing this callback, you will be notified when a navigation event happens and know if it will be blocked (by [`WebPage#navigationLocked`](#webpage-navigationLocked)). Takes the following arguments:
 * `url`: The target URL of this navigation event
 * `type`: Possible values include: `'Undefined'`, `'LinkClicked'`, `'FormSubmitted'`, `'BackOrForward'`, `'Reload'`, `'FormResubmitted'`, `'Other'`
 * `willNavigate`: `true` if navigation will happen, `false` if it is locked (by [`WebPage#navigationLocked`](#webpage-navigationLocked))
 * `main`: `true` if this event comes from the main frame, `false` if it comes from an iframe of some other sub-frame.

Example:
```js
page.onNavigationRequested = function(url, type, willNavigate, main) {
    console.log('Trying to navigate to: ' + url);
    console.log('Caused by: ' + type);
    console.log('Will actually navigate: ' + willNavigate);
    console.log('Sent from the page\'s main frame: ' + main);
}
```

<a name="webpage-onPageCreated" />
#### `onPageCreated` ####
**Introduced:** PhantomJS 1.7  
This callback is invoked when a new child window (but _not_ deeper descendant windows) is created by the page, e.g. using [`window.open`](https://developer.mozilla.org/docs/DOM/window.open). In the PhantomJS outer space, this `WebPage` object will not yet have called its own [`WebPage#open`](#webpage-open) method yet and thus does not yet know its requested URL ([`WebPage#url`](#webpage-url)). Therefore, the most common purpose for utilizing a `WebPage#onPageCreated` callback is to decorate the page (e.g. hook up callbacks, etc.).

Example:
```js
page.onPageCreated = function(newPage) {
    console.log('A new child page was created! Its requested URL is not yet available, though.');
    // Decorate
    newPage.onClosing = function(closingPage) {
        console.log('A child page is closing: ' + closingPage.url);
    };
};
```

<a name="webpage-onPrompt" />
#### `onPrompt` ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `prompt` on the web page. The arguments passed to the callback are the string for the message (`msg`) and the default value (`defaultVal`) for the prompt answer. The return value of the callback handler should be a string.

Example:
```js
page.onPrompt = function(msg, defaultVal) {
    if (msg === 'What's your name?') {
        return 'PhantomJS';
    }
    return defaultVal;
};
```

<a name="webpage-onResourceRequested" />
#### `onResourceRequested` ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page requests a resource. The only argument to the callback is the `request` metadata object.

Example:
```js
page.onResourceRequested = function(request) {
    console.log('Request (#' + request.id + '): ' + JSON.stringify(request));
};
```
The `request` metadata object contains these properties:

 * `id`: the number of the requested resource
 * `method`: http method
 * `url`: the URL of the requested resource
 * `time`: Date object containing the date of the request
 * `headers`: list of http headers

<a name="webpage-onResourceReceived" />
#### `onResourceReceived` ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the a resource requested by the page is received. The only argument to the callback is the `response` metadata object.

If the resource is large and sent by the server in multiple chunks, `onResourceReceived` will be invoked for every chunk received by PhantomJS.

Example:
```js
page.onResourceReceived = function(response) {
    console.log('Response (#' + response.id + ', stage "' + response.stage + '"): ' + JSON.stringify(response));
};
```
The `response` metadata object contains these properties:

 * `id`: the number of the requested resource
 * `url`: the URL of the requested resource
 * `time`: Date object containing the date of the response
 * `headers`: list of http headers
 * `bodySize`: size of the received content (entire content or chunk content)
 * `contentType`: the content type if specified
 * `redirectURL`: if there is a redirection, the redirected URL
 * `stage`: "start", "end" (FIXME: other value for intermediate chunk?)
 * `status`: http status code. ex: `200`
 * `statusText`: http status text. ex: `OK`

<a name="webpage-onUrlChanged" />
#### `onUrlChanged` ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when the URL changes, e.g. as it navigates away from the current URL. The only argument to the callback is the new `targetUrl` string.

Example:
```js
page.onUrlChanged = function(targetUrl) {
    console.log('New URL: ' + targetUrl);
};
```
To retrieve the old URL, use the onLoadStarted callback.

