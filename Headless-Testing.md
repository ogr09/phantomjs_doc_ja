# ヘッドレステスト

PhantomJSの主なユースケースの1つは、Webアプリケーションの **ヘッドレステスト** です。これはプリコミットフック内、および継続的な統合システムの一部として、一般的なコマンドラインベースのテストに適しています。
<!-- One major use case of PhantomJS is **headless testing** of web applications. It is suitable for general command-line based testing, within a precommit hook, and as part of a continuous integration system. -->

## テストフレームワーク
<!-- ## Test Frameworks -->

PhantomJS自体はテストフレームワークでは **ありません** 。PhantomJSは適切なテストランナーを介して、テストを起動するためだけに使用されます。
<!-- PhantomJS itself is **not** a test framework, it is only used to launch the tests via a suitable test runner. -->

次の表は、さまざまテストフレームワークとそれに対応するテストランナーの一覧をまとめています。フレームワークが内部/外部のサードパーティ製ランナーを必要としない場合、 "built-in"としています。
<!-- The following table summarizes the list of various test frameworks and the corresponding test runners. If the framework does not need an external/third-party runner, it is marked as "built-in". -->

| Framework  | Test Runner |
|:-----------|:------------|
| [Buster.JS](http://busterjs.org)| built-in|
| [Capybara](http://jnicklas.github.com/capybara) |[Poltergeist](https://github.com/jonleighton/poltergeist), [Terminus](http://terminus.jcoglan.com)
| [Mocha](http://visionmedia.github.com/mocha) | [mocha-phantomjs](http://metaskills.net/mocha-phantomjs) |
| [FuncUnit](http://funcunit.com) | built-in|
| [Hiro](http://hirojs.com) | built-in|
| [Jasmine](https://github.com/pivotal/jasmine) | [Chutzpah](http://chutzpah.codeplex.com), [grunt-jasmine-runner](https://github.com/jasmine-contrib/grunt-jasmine-runner), [guard-jasmine](https://github.com/netzpirat/guard-jasmine), [phantom-jasmine](https://github.com/jcarver989/phantom-jasmine)|
| [JsTestDriver](http://code.google.com/p/js-test-driver/) | [js-test-driver-phantomjs](https://github.com/larrymyers/js-test-driver-phantomjs) |
| [Robot Framework](http://code.google.com/p/robotframework/) | [phantomrobot](https://github.com/datakurre/phantomrobot)|
| [QUnit](http://qunitjs.com) | [built-in](https://github.com/jquery/qunit/tree/master/addons/phantomjs), [Chutzpah](http://chutzpah.codeplex.com), [JS Test Runner](http://js-testrunner.codehaus.org), [QUnited](http://github.com/aaronroyer/qunited)|
| [Testacular](http://vojtajina.github.com/testacular) | built-in |
| [Testem](https://github.com/airportyh/testem) | built-in |
| [WebDriver](http://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html) | [GhostDriver](https://github.com/detro/ghostdriver)|
| [wru](https://github.com/WebReflection/wru) | built-in|
| [YUITest](http://yuilibrary.com/projects/yuitest) | [Grover](https://github.com/davglass/grover), [phantomjs-yuitest](https://github.com/metafeather/phantomjs-yuitest) |

PhantomJSは`examples`ディレクトリの中に[run-qunit](https://github.com/ariya/phantomjs/blob/master/examples/run-qunit.js)と[run-jasmine](https://github.com/ariya/phantomjs/blob/master/examples/run-jasmine.js)を含んでいます。しかし、これらは実例のためであり、実際に使用する上で必要なレポート機能がありません！
<!-- PhantomJS includes [run-qunit](https://github.com/ariya/phantomjs/blob/master/examples/run-qunit.js) and [run-jasmine](https://github.com/ariya/phantomjs/blob/master/examples/run-jasmine.js) in its `examples` subdirectory. However, these are for illustration purposes and lack important reporting features necessary for real-world uses! -->

## PhantomJS tailored testing

また、テストを目的とした便利で高レベルな機能を提供するためにPhantomJSの上に構築されている[projects](./Related-Projects.md)がいくつかあります:
<!-- In addition, there are [[projects|Related Projects]] which are built on top of PhantomJS to provide convenient high-level functionality for testing purposes: -->

* [Casper.js](http://casperjs.org)はスクリプトナビゲーションとテストを構築するのに有用です
* [Lotte](https://github.com/StanAngeloff/lotte)はjQueryライクなメソッドやチェーン、その他にアサーションロジックを追加しています
* [WebSpecter](https://github.com/jgonera/webspecter)はWebアプリケーションのためのBDDスタイルの受け入れテストのフレームワークです

<!-- * [Casper.js](http://casperjs.org) is useful to build scripted navigation and testing -->
<!-- * [Lotte](https://github.com/StanAngeloff/lotte) adds jQuery-like methods, chaining, and more assertion logic -->
<!-- * [WebSpecter](https://github.com/jgonera/webspecter) is a BDD-style acceptance test framework for web applications -->

## 継続的インテグレーションシステム
<!-- ## Continuous Integration Systems -->

**[Jenkins](http://jenkins-ci.org/)** や **[TeamCity](http://www.jetbrains.com/teamcity/)** といったCIシステムでPhantomJSを使用する場合は、特別なセットアップは必要ありません。PhantomJSがスレーブ/ビルドエージェント上で正しくインストールされていて、実行可能であることを確認してください。
<!-- Using PhantomJS with CI system such as **[Jenkins](http://jenkins-ci.org/)** or **[TeamCity](http://www.jetbrains.com/teamcity/)** does not require special setup. Make sure PhantomJS is installed properly on the slave/build agent and it is ready to go. -->

PhantomJSはLinux上でピュアヘッドレスであるため、エージェントは任意のGUIのインストール上で実行することができます。この方法は、X11のないベアボーンなLinuxシステムでもPhantomJSは問題ありません。これはAmazon EC2やHerokuのインスタンス上で軽量なビルドエージェントを多数起動することが可能になります。
<!-- Since PhantomJS is purely headless on Linux, the agent can run on an installation with any GUI. This means, a barebone Linux system without X11 is not a problem for PhantomJS. It makes it possible to spawn light build agents on Amazon EC2 or Heroku instances. -->

人気ホステッドCIシステムである **[Travis CI](http://about.travis-ci.org/)** は、PhantomJSに組み込みサポートされています。詳細は[its documentation](http://about.travis-ci.org/docs/user/gui-and-headless-browsers/)を参照してください。
<!-- **[Travis CI](http://about.travis-ci.org/)**, a popular hosted CI system, has built-in support for PhantomJS. See [its documentation](http://about.travis-ci.org/docs/user/gui-and-headless-browsers/) for details. -->