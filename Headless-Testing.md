One major use case of PhantomJS is **headless testing** of web applications. It is suitable for general command-line based testing, within a precommit hook, and as part as a continuous integration system.

## Test Frameworks

PhantomJS itself is **not** a test framework, it is only used to launch the tests via a suitable test runner.

The following test frameworks have built-in support for PhantomJS.

* [Buster.JS](http://busterjs.org)
* [FuncUnit](http://funcunit.com)
* [Hiro](http://hirojs.com)
* [QUnit](http://qunitjs.com)
* [Testacular](http://vojtajina.github.com/testacular)

For other test frameworks, various test runners/drivers are usually available:

* [Capybara](http://jnicklas.github.com/capybara/): [Poltergeist](https://github.com/jonleighton/poltergeist)
* [Mocha](http://visionmedia.github.com/mocha/): [mocha-phantomjs](http://metaskills.net/mocha-phantomjs)
* [Jasmine](https://github.com/pivotal/jasmine): [grunt-jasmine-runner](https://github.com/jasmine-contrib/grunt-jasmine-runner), [guard-jasmine](https://github.com/netzpirat/guard-jasmine), [phantom-jasmine](https://github.com/jcarver989/phantom-jasmine)
* [Robot Framework](http://code.google.com/p/robotframework/): [phantomrobot](https://github.com/datakurre/phantomrobot)
* [WebDriver](http://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html): [GhostDriver](https://github.com/detro/ghostdriver)
* [YUITest](http://yuilibrary.com/projects/yuitest/): [Grover](https://github.com/davglass/grover), [phantomjs-yuitest](https://github.com/metafeather/phantomjs-yuitest)

In addition, there are [[projects|Related Projects]] which are built on top of PhantomJS to provide convenient high-level functionality for testing purposes:

* [Casper.js](https://casperjs.org) is useful to build scripted navigation and testing
* [Lotte](https://github.com/StanAngeloff/lotte) adds jQuery-like methods, chaining, and more assertion logic
* [WebSpecter](https://github.com/jgonera/webspecter) is a BDD-style acceptance test framework for web applications

## Continuous Integration Systems

Using PhantomJS with CI system such as **[Jenkins](http://jenkins-ci.org/)** or **[TeamCity](http://www.jetbrains.com/teamcity/)** does not require special setup. Make sure PhantomJS is installed properly on the slave/build agent and it is ready to go.

Since PhantomJS is purely headless on Linux, the agent can run on an installation with any GUI. This means, a barebone Linux system without X11 is not a problem for PhantomJS. It makes it possible to spawn light build agents on Amazon EC2 or Heroku instances.

**[Travis CI](http://about.travis-ci.org/)**, a popular hosted CI system, has built-in support for PhantomJS. See [its documentation](http://about.travis-ci.org/docs/user/gui-and-headless-browsers/) for details.


