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

(To be written)

## Script Arguments

(To be written)

## Page Settings

(To be written)

## Code Evaluation

(To be written)

## Canvas

(To be written)

