_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

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
```js
var server = require('webserver').create();
var Awesome = require('MyAwesomeModule');
Awesome.do();
```
