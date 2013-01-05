### Latest Version

Before reporting an issue, make sure you are using the latest version:

```
phantomjs --version
```

All examples are designed to work with the latest version of PhantomJS. If some examples do not work, make sure that there is not more than one version of PhantomJS installed in the system. Multiple versions may lead to conflict as to which one is being invoked in the terminal.

### SSL and TLS

Transport Layer Security (TLS) and Secure Sockets Layer (SSL) are necessary to access encrypted data, for example when connecting to a server using HTTPS. Thus, if PhantomJS works well with HTTP but it shows some problem when using HTTPS, the first useful thing to check it whether the SSL libraries, usually OpenSSL, have been installed properly.

### Interactive Mode (REPL)

If PhantomJS is launched without any argument, it starts in the so-called interactive mode, also known for [REPL](http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) (read-eval-print-loop). This mode allows a faster cycle of experiment and script prototyping. PhantomJS REPL supports the expected features: command editing, persistent history, and autocomplete (with Tab key).

### Remote Debugging

Remote debugging permits inspection of the script and web page via another WebKit-based browser (Safari and Chrome). This is achieved by launching PhantomJS with the new option, as in this example

```
phantomjs --remote-debugger-port=9000 test.js
```

After than, open Safari/Chrome and go to the http://ipaddress:9000. The browser will show the familiar [Web Inspector interface](http://www.webkit.org/blog/1620/webkit-remote-debugging/) which in this case works on the script being tested.

### Page Error Handler

To easily catch an error occured in a web page, whether it is a syntax error or other thrown exception, an onError handler for the WebPage object has been added. An example on such a handler is:

```javascript
page.onError = function (msg, trace) {
    console.log(msg);
    trace.forEach(function(item) {
        console.log('  ', item.file, ':', item.line);
    })
}
```

Now if the page opens a site with some JavaScript exceptions, a detailed information (including the stack trace) will be printed out.