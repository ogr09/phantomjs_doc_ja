_**Applies to:** PhantomJS 1.7_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

Assuming PhantomJS is [built](http://phantomjs.org/build.html) and its executable is place somewhere in the PATH, it can invoked as follows:
    `phantomjs [options] somescript.js [argument [argument ...]]`

The script code will be executed as if it running in a web browser with an empty page. Since PhantomJS is headless, there will not be anything visible shown up on the screen.

If PhantomJS is invoked without any argument, it will enter the interactive mode (REPL).

<a name="command-line-options" />
## Command-line Options ##
Supported command-line options are:
 * `--cookies-file=/path/to/cookies.txt` specifies the file name to store the persistent cookies.
 * `--disk-cache=[yes|no]` enables disk cache (at desktop services cache storage location, default is `no`).
 * `--help` or `-h` lists all possible command-line options.
 * `--ignore-ssl-errors=[yes|no]` ignores SSL errors, such as expired or self-signed certificate errors (default is `no`).
 * `--load-images=[yes|no]` load all inlined images (default is `yes`).
 * `--local-to-remote-url-access=[yes|no]` allows local content to access remote URL (default is `no`).
 * `--max-disk-cache-size=size` limits the size of disk cache (in KB).
 * `--output-encoding=encoding` sets the encoding used for terminal output (default is `utf8`).
 * `--proxy=address:port` specifies the proxy server to use (e.g. `--proxy=192.168.1.42:8080`).
 * `--proxy-type=[http|socks5]` specifies the type of the proxy server.
 * `--script-encoding=encoding` sets the encoding used for the starting script (default is `utf8`).
 * `--version` or `-v` prints out the version of PhantomJS.
 * `--web-security=[yes|no]` disables web security and allows cross-domain XHR (default is `yes`).

Rather than passing all options in the command-line, it is also possible to store the options in a file using JavaScript Object Notation (JSON) and then tell PhantomJS to read it:
    `phantomjs --config=/path/to/config.json script.js arg1 arg2 arg3`

where the contents of `config.json` looks like:
```javascript
{
    'ignoreSslErrors': true,
    'localToRemoteUrlAccessEnabled': true
}
```

<a name="phantom" />
## Object: `phantom` ##
The interface with various PhantomJS functionalities is carried out using a new host object named `phantom`, added as a child of the [`window` object](https://developer.mozilla.org/en/DOM/window). The properties and functions of the `phantom` object are described in the following sections.

<a name="phantom-properties" />
### Properties ###

<a name="phantom-args" />
#### `args` {array} ####
**Stability:** _DEPRECATED_ - Use [`system.args`](#system-args) from the [System module](#system-module)  
Read-only. An array of the arguments passed to the script.

<a name="phantom-libraryPath" />
#### `libraryPath` {string} ####
This property stores the path which is used by [`injectJs`](#phantom-injectJs) function to resolve the script name. Initially it is set to the location of the script invoked by PhantomJS.

<a name="phantom-scriptName" />
#### `scriptName` {string} ####
**Stability:** _DEPRECATED_ - Use [`system.args[0]`](#system-args) from the [System module](#system-module)  
Read-only. The name of the invoked script file.

<a name="phantom-version" />
#### `version` {object} ####
Read-only. The version of the executing PhantomJS instance. Example value: `{ major: 1, minor: 0, patch: 0 }`.

<a name="phantom-functions" />
### Functions ###

<a name="phantom-exit" />
#### `exit(returnValue)` {void} ####
Exits the program with the specified return value. If no return value is specified, it is set to `0`.

<a name="phantom-injectJs" />
#### `injectJs(filename)` {boolean} ####
Injects external script code from the specified file. If the file cannot be found in the current directory, [`libraryPath`](#phantom-libraryPath) is used for additional look up. This function returns `true` if injection is successful, otherwise it returns `false`.

<a name="module-api" />
# Module API #
The Module API is modeled after [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1.1) is available. Up through PhantomJS 1.6, the only supported modules that were built in:
 * [webpage](#webpage-module)
 * [system](#system-module)
 * [fs](#filesystem-module)
 * [webserver](#webserver-module)

As of PhantomJS 1.7, however, users can reference their own modules from the file system using [`require`](#require) as well.

<a name="require" />
## Function: `require` ##
To support the Module API, a `require` function modeled after [CommonJS Modules' Require](http://wiki.commonjs.org/wiki/Modules/1.1.1#Require) is globally available. General usage:
```javascript
var server = require("webserver").create();
var Awesome = require("MyAwesomeModule");
Awesome.do();
```

<a name="webpage-module" />
## Module: WebPage ##
A `WebPage` object encapsulates a web page. It is usually instantiated using the following pattern:
```javascript
var page = require("webpage").create();
```

**Note:** For backward compatibility with legacy PhantomJS applications, the constructor also remains exposed as a _deprecated_ global `WebPage` object:
```javascript
var page = new WebPage();
```

<a name="webpage-properties" />
### Properties ###

<a name="webpage-clipRect" />
#### `clipRect` {object} ####
This property defines the rectangular area of the web page to be rasterized when [`render()`](#webpage-render) is invoked. If no clipping rectangle is set, [`render()`](#webpage-render) will process the entire web page.

Example:
```javascript
page.clipRect = { top: 14, left: 3, width: 400, height: 300 };
```

<a name="webpage-content" />
#### `content` {string} ####
This property stores the content of the web page, enclosed in HTML/XML element. Setting the property will effectively reload the web page with the new content.

<a name="webpage-libraryPath" />
#### `libraryPath` {string} ####
This property stores the path which is used by [`injectJs`](#webpage-injectJs) function to
resolve the script name. Initially it is set to the location of the
script invoked by PhantomJS.

<a name="webpage-navigationLocked" />
#### `navigationLocked` {boolean} ####
This property defines whether navigation away from the page is permitted or not. If it is set to `true`, then the page is locked to the current URL. Defaults to `false`.

<a name="webpage-paperSize" />
#### `paperSize` {object} ####
This property defines the size of the web page when rendered as a PDF.

The given object should be in one of the following two formats:
````javascript
{ width: '200px', height: '300px', border: '0px' }
{ format: 'A4', orientation: 'portrait', border: '1cm' }
````

If no `paperSize` is defined, the size is defined by the web page. Supported dimension units are: `"mm"`, `"cm"`, `"in"`, `"px"`. No unit means `"px"`. Border is optional and defaults to `0`. Supported formats are: `"A3"`, `"A4"`, `"A5"`, `"Legal"`, `"Letter"`, `"Tabloid"`. Orientation (`"portrait"`, `"landscape"`) is optional and defaults to `"portrait"`.

Example:
```javascript
page.paperSize = { width: "5in", height: "7in", border: "20px" };
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

**Note:** The `settings` apply only during the call to the `page.open(...)` function. Subsequent modification of the `settings` object will not have any impact.


<a name="webpage-viewportSize" />
#### `viewportSize` {object} ####
This property sets the size of the viewport for the layout process. It is useful to set the preferred initial size before loading the page, e.g. to choose between `"landscape"` vs `"portrait"`.</p>

Because PhantomJS is headless (nothing is shown), `viewportSize` effectively simulates the size of the window like in a traditional browser.

Example:
```javascript
page.viewportSize = { width: 480, height: 800 };
```

<a name="webpage-zoomFactor" />
#### `zoomFactor` {number} ####
This property specifies the scaling factor for the `render` and `renderBase64` functions. The default is `1`, i.e. 100% zoom.</p>

Example:
```javascript
// Create a thumbnail preview with 25% zoom
page.zoomFactor = 0.25;
page.render('capture.png');
```

<a name="webpage-customHeaders" />
#### `customHeaders` {object} ####
This property specifies additional HTTP request headers that will be sent to the server for every request issued (for pages _and_ resources). The default value is an empty object `{}`. Headers names and values get encoded in US-ASCII before being sent. Please note that the 'User-Agent' should be set using the <a href="#wiki-webpage-settings">webpage settings</a>, setting the 'User-Agent' property in this property will overwrite the value set via webpage settings</p>

Example:
```javascript
// Send two additional headers 'X-Test' and 'DNT'.
page.customHeaders = {
    'X-Test': 'foo',
    'DNT': '1'
};
```


<a name="webpage-functions" />
### Functions ###

<a name="webpage-close" />
#### `close()` {void} ####
**Introduced:** PhantomJS 1.7  
Close the page and releases the memory heap associated with it. Do not use the page instance after calling this.

Due to some technical limitations, the web page object might not be completely garbage collected. This is often encountered when the same object is used over and over again. Calling this function may stop the increasing heap allocation.

<a name="webpage-evaluate" />
#### `evaluate(function, arg1, arg2, ...)` {object} ####
Evaluates the given function in the context of the web page. The execution is sandboxed, the web page has no access to the `phantom` object and it can't probe its own setting.

Example:
```javascript
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
```javascript
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
Includes external script from the specified `url` (usually a remote location) and executes the `callback` upon completion.

Example:
```javascript
page.includeJs("http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js", function() {
    /* jQuery is loaded, now manipulate the DOM */
});
```

<a name="webpage-injectJs" />
#### `injectJs(filename)` {boolean} ####
Injects external script code from the specified file. If the file cannot be found in the current directory, `libraryPath` is used for additional look up. This function returns `true` if injection is successful, otherwise it returns `false`.

<a name="webpage-open" />
#### `open(url, callback)` {void} ####
Opens the `url` and loads it to the page. Once the page is loaded, the optional `callback` is called using [`page.onLoadFinished`](#webpage-onLoadFinished), and also provides the page status to the function (`"success"` or `"fail"`).

Example:
```javascript
page.open('http://www.google.com/', function(status) {
    console.log("Status: " + status);
    // Do other things here...
});
```

<a name="webpage-release" />
#### `release()` {void} ####
**Stability:** _DEPRECATED_ - Use [`page.close()`](#webpage-close))  
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
#### `sendEvent(type, mouseX, mouseY)` ####
Sends an event to the web page.

The first argument is the event type. Supported type are `"mouseup"`, `"mousedown"`, `"mousemove"`, and `"click"`. The next two arguments are optional but represent the mouse position for the event.

As of now, only the left mouse button is considered pressed for the event. For `"mousemove"`, however, there is no button pressed (i.e. it is not dragging).

The events are not like synthetic [DOM events](http://www.w3.org/TR/DOM-Level-2-Events/events.html). Each event is sent to the web page as if it comes as part of user interaction.

<a name="webpage-uploadFile" />
#### `uploadFile(selector, filename)` ####
Uploads the specified file (`filename`) to the form element associated with the `selector`.

This function is used to automate the upload of a file, which is usually handled with a file dialog in a traditional browser. Since there is no dialog in this headless mode, such an upload mechanism is handled via this special function instead.

Example:
```javascript
page.uploadFile("input[name=image]", "/path/to/some/photo.jpg");
```

<a name="webpage-callbacks" />
### Callbacks ###

<a name="webpage-onAlert" />
#### onAlert ###
**Introduced:** PhantomJS 1.0  
This callback is invoked when there is a JavaScript `alert` on the web page. The only argument passed to the callback is the string for the message. There is no return value expected from the callback handler.

Example:
```javascript
page.onAlert = function(msg) {
    console.log("ALERT: " + msg);
};
```

<a name="webpage-onCallback" />
#### onCallback ####
**Stability:** _EXPERIMENTAL_
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `window.callPhantom` call made on the web page. The only argument passed to the callback is a data object.

Although there are many possible use cases for this inversion of control, the primary one so far is to prevent the need for a PhantomJS script to be continually polling for some variable on the web page.

Example:
```javascript
page.onCallback = function(data) {
    console.log("CALLBACK: " + JSON.stringify(data));
};
```

<a name="webpage-onClosing" />
#### onClosing ####
**Introduced:** PhantomJS 1.7  
This callback is invoked whenever the top-level page or one of its child pages (e.g. created via [`window.open`](https://developer.mozilla.org/docs/DOM/window.open)) are closed.  It takes one argument, which is a reference to the page that is closing.

Example:
```javascript
page.onClosing = function(closingPage) {
    if (closingPage === page) {
        console.log("The top-level page is closing!");
    }
	else {
	    console.log("A child page is closing.");
	}
};
```

<a name="webpage-onConfirm" />
#### onConfirm ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `confirm` on the web page. The only argument passed to the callback is the string for the message. The return value of the callback handler can be either `true` or `false`, which are equivalent to pressing the "OK" or "Cancel" buttons presented in a JavaScript `confirm`, respectively.

Example:
```javascript
page.onConfirm = function(msg) {
    console.log("CONFIRM: " + msg);
	return true;  // `true` === pressing the "OK" button, `false` === pressing the "Cancel" button
};
```

<a name="webpage-onConsoleMessage" />
#### onConsoleMessage ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when there is a JavaScript `console` message on the web page. The callback may accept up to three arguments: the string for the message, the line number, and the source identifier.

By default, `console` messages from the web page are not displayed. Using this callback is a typical way to redirect it.

Example:
```javascript
page.onConsoleMessage = function(msg, lineNum, sourceId) {
    console.log("CONSOLE: " + msg + ' (from line #' + lineNum + ' in "' + sourceId + '")');
};
```

<a name="webpage-onError" />
#### onError ####
**Introduced:** PhantomJS 1.5  
This callback is invoked when there is a JavaScript execution error. It is a good way to catch problems when evaluating a script in the web page context. The arguments passed to the callback are the error message and the stack trace (as an Array).

Example:
```javascript
page.onError = function(msg, trace) {
	var msgStack = ["ERROR: " + msg];
	if (trace) {
		msgStack.push("TRACE:");
		trace.forEach(function(t) {
			msgStack.push(" -> " + t.file + ": " + t.line + (t.function ? " (in function '" + t.function + "')" : ""));
		});
	}
	console.error(msgStack.join("\n"));
};
```

<a name="webpage-onInitialized" />
#### onInitialized ####
**Introduced:** PhantomJS 1.3  
This callback is invoked _after_ the web page is created but _before_ a URL is loaded. The callback may be used to change global objects.

Example:
```javascript
page.onInitialized = function() {
    page.evaluate(function() {
        document.addEventListener("DOMContentLoaded", function() {
			console.log("DOM content has loaded.");
		}, false);
    });
};
```

<a name="webpage-onLoadFinished" />
#### onLoadFinished ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page finishes the loading. It may accept a single argument indicating the page's `status`: `"success"` if no network errors occurred, otherwise `"fail"`.

Also see [`page.open`](#webpage-open) for an alternate hook for the `onLoadFinished` callback.

Example:
```javascript
page.onLoadFinished = function(status) {
    console.log("Status: " + status);
	// Do other things here...
};
```

<a name="webpage-onLoadStarted" />
#### onLoadStarted ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page starts the loading. There is no argument passed to the callback.

Example:
```javascript
page.onLoadStarted = function() {
    console.log("Now loading...");
};
```

<a name="webpage-onNavigationRequested" />
#### onNavigationRequested ####
**Introduced:** PhantomJS 1.6  
By implementing this callback, you will be notified when a navigation event happens and know if it will be blocked (by `page.navigationLocked`). Takes the following arguments:
 * `url`: The target URL of this navigation event
 * `type`: Possible values include: `"Undefined"`, `"LinkClicked"`, `"FormSubmitted"`, `"BackOrForward"`, `"Reload"`, `"FormResubmitted"`, `"Other"`
 * `willNavigate`: `true` if navigation will happen, `false` if it is locked (by `page.navigationLocked`)
 * `main`: `true` if this events comes from the main frame, `false` if it comes from an iframe of some other sub-frame.

Example:
```javascript
page.onNavigationRequested = function(url, type, willNavigate, main) {
    console.log("Trying to navigate to: " + url);
    console.log("Caused by: " + type);
	console.log("Will actually navigate: " + willNavigate);
	console.log("Sent from the page's main frame: " + main);
}
```

<a name="webpage-onPrompt" />
#### onPrompt ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when there is a JavaScript `prompt` on the web page. The arguments passed to the callback are the string for the message (`msg`) and the default value (`defaultVal`) for the prompt answer. The return value of the callback handler should be a string.

Example:
```javascript
page.onPrompt = function(msg, defaultVal) {
    if (msg === "What's your name?") {
	    return "PhantomJS";
	}
	return defaultVal;
};
```

<a name="webpage-onResourceRequested" />
#### onResourceRequested ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the page requests a resource. The only argument to the callback is the `request` metadata object.

Example:
```javascript
page.onResourceRequested = function(request) {
    console.log("Request (#" + request.id + "): " + JSON.stringify(request));
};
```

<a name="webpage-onResourceReceived" />
#### onResourceReceived ####
**Introduced:** PhantomJS 1.2  
This callback is invoked when the a resource requested by the page is received. The only argument to the callback is the `response` metadata object.

If the resource is large and sent by the server in multiple chunks, `onResourceReceived` will be invoked for every chunk received by PhantomJS.

Example:
```javascript
page.onResourceReceived = function(response) {
    console.log('Response (#' + response.id + ', stage "' + response.stage + '"): ' + JSON.stringify(response));
};
```

<a name="webpage-onUrlChanged" />
#### onUrlChanged ####
**Introduced:** PhantomJS 1.6  
This callback is invoked when the URL changes, e.g. as it navigates away from the current URL. The only argument to the callback is the new `targetUrl` string.

Example:
```javascript
page.onUrlChanged = function(targetUrl) {
    var currentUrl = page.evaluate(function() {
		return window.location.href;
	});
	console.log("Old URL: " + currentUrl);
	console.log("New URL: " + targetUrl);
};
```

<a name="system-module" />
## Module: System ##
A set of functions to access system-level functionalities is available, modeled after the [CommonJS System proposal](http://wiki.commonjs.org/wiki/System).

To start using, you must `require` a reference to the `system` module:
```javascript
var system = require('system');
```

<a name="system-properties" />
### Properties ###

<a name="system-platform" />
#### platform {string} ####
Read-only. The name of the platform, for which the value is always `"phantomjs"`.

<a name="system-os" />
#### os {object} ####
Read-only. An object providing information about the operating system, including `architecture`, `name`, and `version`.

<a name="system-env" />
#### env {object} ####
Queries and returns a list of key-value pairs representing the environment variables.

The following example demonstrates the same functionlity as the Unix `printenv` utility or the Windows `set` command:
```javascript
var env = require("system").env;
Object.keys(env).forEach(function(key) {
    console.log(key + "=" + env[key]);
});
```

<a name="system-args" />
#### args {array} ####
Queries and returns a list of the command-line arguments.  The first one is always the script name, which is then followed by the subsequent arguments.

The following example prints all of the command-line arguments:
```javascript
var args = require("system").args;
if (args.length === 1) {
    console.log("Try to pass some arguments when invoking this script!");
}
else {
    args.forEach(function(arg, i) {
        console.log(i + ": " + arg);
    });
}
```

<a name="filesystem-module" />
## Module: FileSystem ##
A set of API functions is available to access files and directories, modeled after the [CommonJS Filesystem proposal](http://wiki.commonjs.org/wiki/Filesystem).

To start using, you must `require` a reference to the `fs` module:
```javascript
var fs = require("fs");
```

<a name="filesystem-properties" />
### Properties ###

<a name="filesystem-separator" />
#### separator {string} ####
Read-only. The path separator (`/` or `\`, depending on the operating system).

<a name="filesystem-workingDirectory" />
#### workingDirectory {string} ####
Read-only. The current working directory.

<a name="filesystem-functions" />
### Functions ###

<a name="filesystem-query-functions" />
#### Query Functions ####
 * `list(path)`: Returns the list of all the files in the specified `path`.
 * `absolute(path)`: Returns the absolute path starting from the root file system, resolved from the current `workingDirectory`.
 * `exists(path)`: Returns `true` if a file or a directory exists.
 * `isDirectory(path)`: Returns `true` if the specified `path` is a directory.
 * `isFile(path)`: Returns `true` if the specified `path` is a file.
 * `isAbsolute(path)`: Returns `true` if the specified `path` is an absolute path.
 * `isExecutable(path)`: Returns `true` if the specified file can be executed.
 * `isReadable(path`: Returns `true` if a file or a directory is readable.
 * `isWritable(path)`: Returns `true` if a file or a directory is writable.
 * `isLink(path)`: Returns `true` if the specified `path` is a symbolic link.
 * `readLink(path)`: Returns the target of a symbolic link.

<a name="filesystem-directory-functions" />
#### Directory Functions ####
 * `changeWorkingDirectory(path)`: Changes the current `workingDirectory` to the specified `path`.
 * `makeDirectory(path)`: Creates a new directory.
 * `makeTree(path)`: Creates a directory including any missing parent directories.
 * `removeDirectory(path)`: Removes a directory if it is empty
 * `removeTree(path)`: Removes the specified `path`, regardless of whether it is a file or a directory.
 * `copyTree(source, destination)`: Copies all files from the `source` path to the `destination` path.

<a name="filesystem-file-functions" />
#### File Functions ####
 * `open(path, mode)`: Returns a `stream` object representing the stream interface to the specified file (`mode` can be `"r"` for read, `"w"` for write, or `"a"` for append).
 * `read(path)`: Returns the entire content of a file.
 * `write(path, content, mode)`: Writes content to a file (`mode` can be `"w"` for write or `"a"` for append).
 * `size(path)`: Returns the size (in bytes) of the file specified by the `path`.
 * `remove(path)`: Removes the file specified by the `path`.
 * `copy(source, destination)`: Copies a file to another.
 * `move(source, destination)`: Moves a file to another, effectively renaming it.
 * `touch(path)`: Touches a file, changing its access timestamp.

<a name="filesystem-stream" />
#### `stream` object ####
A `stream` object returned from the `fs.open(...)` function has the following functions:
 * `read()`: Returns the content of the stream.
 * `readLine()`: Reads only a line from the stream and return it.
 * `write(data)`: Writes the string to the stream.
 * `writeLine(data)`: Writes the data as a line to the stream.
 * `flush()`: Flushes all pending input output.
 * `close()`: Completes the stream operation.

<a name="webserver-module" />
## Module: WebServer ##
**Stability:** _EXPERIMENTAL_  
**Introduced:** PhantomJS 1.4  
Using an embedded web server module called [Mongoose](http://code.google.com/p/mongoose/), PhantomJS script can start a web server. This is intended for ease of communication between PhantomJS scripts and the outside world and is _not_ recommended for use as a general production server.

Here is a simple example that always gives the same response regardless of the request:
```javascript
var server = require('webserver').create();
var service = server.listen(8080, function(request, response) {
    response.statusCode = 200;
    response.write('<html><body>Hello!</body></html>');
    response.close();
});
```

<a name="webserver-request" />
The `request` object passed to the callback function may contain the following properties:
 * `method`: Defines the request method (`"GET"`, `"POST"`, etc.)
 * `url`: The complete request URL, including the query string (if any)
 * `httpVersion`: The actual HTTP version
 * `headers`: All of the HTTP headers as key-value pairs
 * `post`: The request body (only for `"POST"` and `"PUT"` method requests)
 * `postRaw`: If the `Content-Type` header is set to `"application/x-www-form-urlencoded"` (the default for form submissions), the original contents of `post` will be stored in this extra property (`postRaw`) and then `post` will be automatically updated with a URL-decoded version of the data.
 
<a name="webserver-response" />
The `response` object should be used to create the response using the following properties and functions:
 * `headers`: Stores all HTTP headers as key-value pairs. These must be set **BEFORE** calling `write` for the first time.
 * `statusCode`: Sets the returned HTTP status code.
 * `write(data)`: Sends a chunk for the response body. Can be called multiple times.
 * `close()`: Closes the HTTP connection.
      * To avoid the client detecting a connection drop, remember to use `write()` at least once. Sending an empty string (i.e. `write("")`) would be enough if the only aim is, for example, to return an HTTP status code of `200` (`"OK"`).