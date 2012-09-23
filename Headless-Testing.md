One major use case of PhantomJS is **headless testing** of web applications. It is suitable for general command-line based testing, within a precommit hook, and as part as a continuous integration system.

## Test Frameworks

PhantomJS itself is **not** a test framework, it is only used to launch the tests via a suitable test runner.

The following test frameworks have built-in support for PhantomJS.

* [Buster.JS](http://busterjs.org)
* [FuncUnit](http://funcunit.com)
* [Hiro](http://hirojs.com)
* [QUnit](http://qunitjs.com)

For other test frameworks, various test runners/drivers are usually available:

* [Capybara](http://jnicklas.github.com/capybara/): [Poltergeist](https://github.com/jonleighton/poltergeist)
* [Mocha](http://visionmedia.github.com/mocha/): [mocha-phantomjs](http://metaskills.net/mocha-phantomjs)
* [Jasmine](https://github.com/pivotal/jasmine): [grunt-jasmine-runner](https://github.com/jasmine-contrib/grunt-jasmine-runner), [guard-jasmine](https://github.com/netzpirat/guard-jasmine), [phantom-jasmine](https://github.com/jcarver989/phantom-jasmine)
* [Robot Framework](http://code.google.com/p/robotframework/): [phantomrobot](https://github.com/datakurre/phantomrobot)
* [WebDriver](http://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html): [GhostDriver](https://github.com/detro/ghostdriver)
* [YUITest](http://yuilibrary.com/projects/yuitest/): [Grover](https://github.com/davglass/grover), [phantomjs-yuitest](https://github.com/metafeather/phantomjs-yuitest)


