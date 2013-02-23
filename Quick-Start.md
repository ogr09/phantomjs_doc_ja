# クイックスタート

このインストラクションは、PhantomJSがインストールされており、その実行ファイルがどこかPATHの通った場所に配置されていることを前提としています。
<!-- This instruction assumes that PhantomJS is installed and its executable is placed somewhere in the PATH. -->

ここで示すコードは、PhantomJSが含まれている[various examples](./Examples.md)でも利用できます。また、[page automation](./Page-Automation.md)や[network monitoring](Network-Monitoring.md)、[screen capture](Screen-Capture.md)、[headless testing](Headless-Testing.md)でPhantomJSの使い方を探すこともお勧めします。
<!-- The code shown here is also available in [[various examples|Examples]] included with PhantomJS. You are also recommended to explore the use of PhantomJS for [[page automation|Page Automation]], [[network monitoring|Network Monitoring]], [[screen capture|Screen Capture]], and [[headless testing|Headless Testing]]. -->

## Hello, World!

次の2行を含む新しいテキストフ​​ァイルを作成します:
<!-- Create a new text file that contains the following two lines: -->

```javascript
console.log('Hello, world!');
phantom.exit();
```

`hello.js`として保存し、実行します:
<!-- Save it as `hello.js` and then run it: -->

    phantomjs hello.js

出力は次のとおりです:
<!-- The output is: -->

> Hello, world!

最初の行の`console.log`は渡された文字列を端末に出力します。2行目の`phantom.exit`は実行を終了します。
<!-- In the first line, `console.log` will print the passed string to the terminal. In the second line, `phantom.exit` terminates the execution. -->

スクリプトのどこかで`phantom.exit`を呼び出すことは **とても重要** です。もし`phantom.exit`を呼び出さないとPhantomJSは終了しません。
<!-- It is **very important** to call `phantom.exit` at some point in the script, otherwise PhantomJS will not be terminated at all. -->

## ページの読み込み
<!-- ## Page Loading -->

Webページのオブジェクトが作成されることで、Webページは読み込まれ、分析され、レンダリングされます。
<!-- web page can be loaded, analyzed, and rendered by creating a web page object. -->

次のスクリプトは、ページオブジェクトの最も簡単な使用方法を示しています。Googleのホームページを読み込み、それを画像として`google.png`で保存します。
<!-- The following script demonstrates the simplest use of page object. It loads Google homepage and then save it as an image, `google.png`. -->

```javascript
var page = require('webpage').create();
page.open('http://google.com', function () {
    page.render('google.png');
    phantom.exit();
});
```

そのレンダリング機能があるため、PhantomJSはコンテンツのスクリーンショットを撮るような[capture web pages](Screen-Capture.md)に使用できます。
<!-- Because of its rendering features, PhantomJS can be used to [[capture web pages|Screen Capture]], essentially taking a screenshot of the contents. -->

以下の`loadspeed.js`スクリプトは、指定されたURL（httpプロトコルを忘れないでください）を読み込み、それを読み込むのに必要な時間を測定します。
<!-- The following `loadspeed.js` script loads a specified URL (do not forget the http protocol) and measures the time it takes to load it. -->

```javascript
var page = require('webpage').create(),
    system = require('system'),
    t, address;

if (system.args.length === 1) {
    console.log('Usage: loadspeed.js <some URL>');
    phantom.exit();
}

t = Date.now();
address = system.args[1];
page.open(address, function (status) {
    if (status !== 'success') {
        console.log('FAIL to load the address');
    } else {
        t = Date.now() - t;
        console.log('Loading time ' + t + ' msec');
    }
    phantom.exit();
});
```

コマンドでスクリプトを実行します:
<!-- Run the script with the command: -->

    phantomjs loadspeed.js http://www.google.com

出力は次のようになります：
<!-- It outputs something like: -->

> Loading http://www.google.com
> Loading time 719 msec

## コー​​ドの評価
<!-- ## Code Evaluation -->

Webページのコンテキスト内のJavaScriptやCoffeeScriptのコードを評価するためには、`evaluate()`関数を使います。実行は"サンドボックス化"され、自身のページのコンテキスト外から任意のJavaScriptオブジェクトや変数にアクセスするための方法はありません。オブジェクトは`evaluate()`から返されるが、単純なオブジェクトに制限されており、関数やクロージャを含めることはできません。
<!-- To evaluate JavaScript or CoffeeScript code in the context of the web page, use `evaluate()` function. The execution is "sandboxed", there is no way for the code to access any JavaScript objects and variables outside its own page context. An object can be returned from `evaluate()`, however it is limited to simple objects and can't contain functions or closures. -->

次にWebページのタイトルを表示する例を示します:
<!-- Here is an example to show the title of a web page: -->

```javascript
var page = require('webpage').create();
page.open(url, function (status) {
    var title = page.evaluate(function () {
        return document.title;
    });
    console.log('Page title is ' + title);
});
```

`evaluate()`の内部コードからのメッセージを含むWebページからの任意のコンソールメッセージは、デフォルトでは表示されません。この動作をオーバーライドするには、`onConsoleMessage`コールバックを使います。前の例は次のように書き換えることができます。
<!-- Any console message from a web page, including from the code inside `evaluate()`, will not be displayed by default. To override this behavior, use the `onConsoleMessage` callback. The previous example can be rewritten to: -->

```javascript
var page = require('webpage').create();
page.onConsoleMessage = function (msg) {
    console.log('Page title is ' + msg);
};
page.open(url, function (status) {
    page.evaluate(function () {
        console.log(document.title);
    });
});
```

Webブラウザ上で実行されているかのようにスクリプトが実行されているので、標準のDOMスクリプティングとCSSセレクタは問題なく動作します。そのため、PhantomJSは様々な[page automation tasks](Page-Automation.md)を処理するのに適しています。
<!-- Since the script is executed as if it is running on a web browser, standard DOM scripting and CSS selectors work just fine. It makes PhantomJS suitable to carry out various [[page automation tasks|Page Automation]]. -->

## ネットワーク要求と応答
<!-- ## Network Requests and Responses -->

ページがリモートサーバからリソースを要求する時、要求と応答の両方を`onResourceRequested`と`onResourceReceived`コールバックを介して追跡することができます。これは[netlog.js](https://github.com/ariya/phantomjs/blob/master/examples/netlog.js)の例に示されています：
<!-- When a page requests a resource from a remote server, both the request and the response can be tracked via `onResourceRequested` and `onResourceReceived` callback. This is demonstrated in the example [netlog.js](https://github.com/ariya/phantomjs/blob/master/examples/netlog.js): -->

```javascript
var page = require('webpage').create();
page.onResourceRequested = function (request) {
    console.log('Request ' + JSON.stringify(request, undefined, 4));
};
page.onResourceReceived = function (response) {
    console.log('Receive ' + JSON.stringify(response, undefined, 4));
};
page.open(url);
```

このHARエクスポートの機能や、YSlow-basedのパフォーマンス分析を活用する詳細な方法は、[network monitoring](Network-Monitoring.md)を参照してください。
<!-- For more info on how to utilize this features for HAR export as well as YSlow-based performance analysis, see the page on [[network monitoring|Network Monitoring]]. -->