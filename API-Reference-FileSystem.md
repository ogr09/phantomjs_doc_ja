_**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_

**Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]].

<a name="filesystem-module" />
## Module: FileSystem ##
A set of API functions is available to access files and directories, modeled after the [CommonJS Filesystem proposal](http://wiki.commonjs.org/wiki/Filesystem).

To start using, you must `require` a reference to the `fs` module:
```js
var fs = require('fs');
```

<a name="filesystem-properties" />
### Properties ###

<a name="filesystem-separator" />
#### `separator` {String} ####
Read-only. The path separator (`/` or `\`, depending on the operating system).

<a name="filesystem-workingDirectory" />
#### `workingDirectory` {String} ####
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
 * `open(path, mode)`: Returns a `stream` object representing the stream interface to the specified file (`mode` can be `'r'` for read, `'w'` for write, or `'a'` for append).
 * `read(path)`: Returns the entire content of a file.
 * `write(path, content, mode)`: Writes content to a file (`mode` can be `'w'` for write or `'a'` for append).
 * `size(path)`: Returns the size (in bytes) of the file specified by the `path`.
 * `remove(path)`: Removes the file specified by the `path`.
 * `copy(source, destination)`: Copies a file to another.
 * `move(source, destination)`: Moves a file to another, effectively renaming it.
 * `touch(path)`: Touches a file, changing its access timestamp.

<a name="filesystem-stream" />
#### `stream` object ####
A `stream` object returned from the `fs.open` function has the following methods:
 * `read()`: Returns the content of the stream.
 * `readLine()`: Reads only a line from the stream and return it.
 * `write(data)`: Writes the string to the stream.
 * `writeLine(data)`: Writes the data as a line to the stream.
 * `flush()`: Flushes all pending input/output.
 * `close()`: Completes the stream operation.
