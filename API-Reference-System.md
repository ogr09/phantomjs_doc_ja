_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

<a name="system-module" />
## Module: System ##
A set of functions to access system-level functionality, modeled after the [CommonJS System proposal](http://wiki.commonjs.org/wiki/System). This also includes process-related properties.

To start using, you must `require` a reference to the `system` module:
```js
var system = require('system');
```

<a name="system-properties" />
### Properties ###

<a name="system-pid" />
#### `pid` {Number} ####
**Introduced:** PhantomJS 1.8  
Read-only. The PID (Process ID) for the currently executing PhantomJS process.

<a name="system-platform" />
#### `platform` {String} ####
Read-only. The name of the platform, for which the value is always `'phantomjs'`.

<a name="system-os" />
#### `os` {Object} ####
Read-only. An object providing information about the operating system, including `architecture`, `name`, and `version`. For example:
```js
var os = require('system').os;
console.log(os.architecture);  // '32bit'
console.log(os.name);  // 'windows'
console.log(os.version);  // '7'
```

<a name="system-env" />
#### `env` {Object} ####
Queries and returns a list of key-value pairs representing the environment variables.

The following example demonstrates the same functionality as the Unix `printenv` utility or the Windows `set` command:
```js
var env = require('system').env;
Object.keys(env).forEach(function(key) {
    console.log(key + '=' + env[key]);
});
```

<a name="system-args" />
#### `args` {String[]} ####
Queries and returns a list of the command-line arguments.  The first one is always the script name, which is then followed by the subsequent arguments.

The following example prints all of the command-line arguments:
```js
var args = require('system').args;
if (args.length === 1) {
    console.log('Try to pass some arguments when invoking this script!');
}
else {
    args.forEach(function(arg, i) {
        console.log(i + ': ' + arg);
    });
}
```
