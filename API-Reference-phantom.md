_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

<a name="phantom" />
## Object: `phantom` ##
The interface with various PhantomJS functionalities is carried out using a new host object named `phantom`, added as a child of the [`window` object](https://developer.mozilla.org/en/DOM/window). The properties and functions of the `phantom` object are described in the following sections.

<a name="phantom-properties" />
### Properties ###

<a name="phantom-args" />
#### `args` {String[]} ####
**Stability:** _DEPRECATED_ - Use [`system.args`](#system-args) from the [System module](#system-module)  
Read-only. An array of the arguments passed to the script.

<a name="phantom-cookies" />
#### `cookies` {[Cookie](#cookie)[]} ####
**Introduced:** PhantomJS 1.7  
Get or set [Cookies](#cookie) for any domain (though, for setting, use of [`phantom.addCookie`](#phantom-addCookie) is preferred). These Cookies are stored in the CookieJar and will be supplied when opening pertinent WebPages. This array will be pre-populated by any existing Cookie data stored in the cookie file specified in the PhantomJS [startup config/command-line options](#command-line-options), if any.

<a name="phantom-cookiesEnabled" />
#### `cookiesEnabled` {Boolean} ####
**Introduced:** PhantomJS 1.7  
Controls whether the CookieJar is enabled or not.  Defaults to `true`.

<a name="phantom-libraryPath" />
#### `libraryPath` {String} ####
This property stores the path which is used by [`injectJs`](#phantom-injectJs) function to resolve the script name. Initially it is set to the location of the script invoked by PhantomJS.

<a name="phantom-scriptName" />
#### `scriptName` {String} ####
**Stability:** _DEPRECATED_ - Use [`system.args[0]`](#system-args) from the [System module](#system-module)  
Read-only. The name of the invoked script file.

<a name="phantom-version" />
#### `version` {Object} ####
Read-only. The version of the executing PhantomJS instance. Example value: `{ 'major': 1, 'minor': 7, 'patch': 0 }`.

<a name="phantom-functions" />
### Functions ###

<a name="phantom-addCookie" />
#### `addCookie([Cookie](#cookie))` {Boolean} ####
**Introduced:** PhantomJS 1.7  
Add a [Cookie](#cookie) to the CookieJar.  Returns `true` if successfully added, otherwise `false`. See [`phantom.cookies`](#phantom-cookies) for more information on the CookieJar.

Example:
```js
phantom.addCookie({
    'name': 'Added-Cookie-Name',
    'value': 'Added-Cookie-Value',
    'domain': '.google.com'
});
```


<a name="phantom-clearCookies" />
#### `clearCookies()` {void} ####
**Introduced:** PhantomJS 1.7  
Delete all [Cookies](#cookie) in the CookieJar. See [`phantom.cookies`](#phantom-cookies) for more information on the CookieJar.

<a name="phantom-deleteCookie" />
#### `deleteCookie(cookieName)` {Boolean} ####
**Introduced:** PhantomJS 1.7  
Delete any [Cookies](#cookie) in the CookieJar with a '`name'` property matching `cookieName`. Returns `true` if successfully deleted, otherwise `false`. See [`phantom.cookies`](#phantom-cookies) for more information on the CookieJar.

Example:
```js
phantom.deleteCookie('Added-Cookie-Name');
```


<a name="phantom-exit" />
#### `exit(returnValue)` {void} ####
Exits the program with the specified return value. If no return value is specified, it is set to `0`.

<a name="phantom-injectJs" />
#### `injectJs(filename)` {boolean} ####
Injects external script code from the specified file into the Phantom outer space. If the file cannot be found in the current directory, [`libraryPath`](#phantom-libraryPath) is used for additional look up. This function returns `true` if injection is successful, otherwise it returns `false`.


<a name="phantom-callbacks" />
### Callbacks ###

<a name="phantom-onError" />
#### `onError` ####
**Introduced:** PhantomJS 1.5  
This callback is invoked when there is a JavaScript execution error _not_ caught by a [`WebPage#onError`](#webpage-onError) handler. This is the closest it gets to having a global error handler in PhantomJS, and so it is a best practice to set this `onError` handler up in order to catch any unexpected problems. The arguments passed to the callback are the error message and the stack trace [as an Array].

Example:
```js
phantom.onError = function(msg, trace) {
    var msgStack = ['PHANTOM ERROR: ' + msg];
    if (trace) {
        msgStack.push('TRACE:');
        trace.forEach(function(t) {
            msgStack.push(' -> ' + (t.file || t.sourceURL) + ': ' + t.line + (t.function ? ' (in function ' + t.function + ')' : ''));
        });
    }
    console.error(msgStack.join('\n'));
};
```