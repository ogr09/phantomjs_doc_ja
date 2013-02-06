_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

Assuming PhantomJS is [built](http://phantomjs.org/build.html) and its executable is place somewhere in the PATH, it can be invoked as follows:
    `phantomjs [options] somescript.js [arg1 [arg2 [...]]]`

The script code will be executed as if it running in a web browser with an empty page. Since PhantomJS is headless, there will not be anything visible shown up on the screen.

If PhantomJS is invoked without any argument, it will enter the interactive mode (REPL).

<a name="command-line-options" />
## Command-line Options ##
Supported command-line options are:
 * `--help` or `-h` lists all possible command-line options. _Halts immediately, will not run a script passed as argument._
 * `--version` or `-v` prints out the version of PhantomJS. _Halts immediately, will not run a script passed as argument._
 * `--cookies-file=/path/to/cookies.txt` specifies the file name to store the persistent [Cookies](#cookie).
 * `--disk-cache=[true|false]` enables disk cache (at desktop services cache storage location, default is `false`). Also accepted: `[yes|no]`.
 * `--ignore-ssl-errors=[true|false]` ignores SSL errors, such as expired or self-signed certificate errors (default is `false`). Also accepted: `[yes|no]`.
 * `--load-images=[true|false]` load all inlined images (default is `true`). Also accepted: `[yes|no]`.
 * `--local-to-remote-url-access=[true|false]` allows local content to access remote URL (default is `false`). Also accepted: `[yes|no]`.
 * `--max-disk-cache-size=size` limits the size of disk cache (in KB).
 * `--output-encoding=encoding` sets the encoding used for terminal output (default is `utf8`).
 * `--proxy=address:port` specifies the proxy server to use (e.g. `--proxy=192.168.1.42:8080`).
 * `--proxy-type=[http|socks5|none]` specifies the type of the proxy server (default is `http`).
 * `--script-encoding=encoding` sets the encoding used for the starting script (default is `utf8`).
 * `--ssl-protocol=[sslv3|sslv2|tlsv1|any']` sets the SSL protocol for secure connections (default is `SSLv3`).
 * `--web-security=[true|false]` enables web security and forbids cross-domain XHR (default is `true`). Also accepted: `[yes|no]`.

Alternatively, since PhantomJS 1.3, you can also utilize a JavaScript Object Notation (JSON) configuration file instead of passing in multiple command-line options:
 * `--config=/path/to/config.json`

The contents of `config.json` should be a standalone JavaScript object. Keys are de-dashed, camel-cased equivalents of the other supported command-line options (excluding `--version`/`-v` and `--help`/`-h`).  Values are their JavaScript equivalents: 'true'/'false' (or 'yes'/'no') values translate into `true`/`false` Boolean values, numbers remain numbers, strings remain strings. For example:
```js
{
    /* Same as: --ignore-ssl-errors=true */
    "ignoreSslErrors": true,

    /* Same as: --max-disk-cache-size=1000 */
    "maxDiskCacheSize": 1000,

    /* Same as: --output-encoding=utf8 */
    "outputEncoding": "utf8"

    /* etc. */
}
```

<a name="phantom" />
## Object: `phantom` ##
[[See dedicated page |API-Reference-phantom]]



<a name="module-api" />
# Module API #
The Module API is modeled after [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1.1) is available. Up through PhantomJS 1.6, the only supported modules that were built in:
 * [webpage](API-Reference-WebPage)
 * [system](API-Reference-System)
 * [fs](API-Reference-FileSystem)
 * [webserver](API-Reference-WebServer)

As of PhantomJS 1.7, however, users can reference their own modules from the file system using [`require`](#require) as well.

<a name="require" />
## Function: `require` ##
To support the Module API, a `require` function modeled after [CommonJS Modules' Require](http://wiki.commonjs.org/wiki/Modules/1.1.1#Require) is globally available. General usage:
```js
var server = require('webserver').create();
var Awesome = require('MyAwesomeModule');
Awesome.do();
```

# Appendix #
<a name="cookie" />
## `Cookie` {Object} ##
Various methods in the [`phantom`](#phantom) object, as well as in [`WebPage`](#webpage) instances, utilize `Cookie` objects. These are best created via object literals.

For example:
```js
phantom.addCookie({
    'name':     'Valid-Cookie-Name',   /* required property */
    'value':    'Valid-Cookie-Value',  /* required property */
    'domain':   'localhost',           /* required property */
    'path':     '/foo',
    'httponly': true,
    'secure':   false,
    'expires':  (new Date()).getTime() + 3600   /* <- expires in 1 hour */
});
```