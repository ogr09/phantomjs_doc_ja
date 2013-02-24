# ページの自動化

PhantomJSはWebページを読み込み操作することができるので、様々なページの自動化をおこなうことができます。
<!-- Because PhantomJS can load and manipulate a web page, it is perfect to carry out various page automations. -->

## DOM操作
<!-- ## DOM Manipulation -->

Webブラウザ上で実行されているかのようにスクリプトが実行されるので、標準の[DOM scripting](http://en.wikipedia.org/wiki/DOM_scripting)と[CSS selectors](http://www.w3.org/TR/css3-selectors/)は問題なく動作します。
<!-- Since the script is executed as if it is running on a web browser, standard [DOM scripting](http://en.wikipedia.org/wiki/DOM_scripting) and [CSS selectors](http://www.w3.org/TR/css3-selectors/) work just fine. -->

以下の`useragent.js`の例では、 *id* が`myagent`である要素の`innerText`プロパティの読み込みを示しています:
<!-- The following `useragent.js` example demonstrates reading the `innerText` property of the element whose *id* is `myagent`: -->

```javascript
var page = require('webpage').create();
console.log('The default user agent is ' + page.settings.userAgent);
page.settings.userAgent = 'SpecialAgent';
page.open('http://www.httpuseragent.org', function (status) {
    if (status !== 'success') {
        console.log('Unable to access network');
    } else {
        var ua = page.evaluate(function () {
            return document.getElementById('myagent').innerText;
        });
        console.log(ua);
    }
    phantom.exit();
});
```

上記の例では、リモートWebサーバーによって認識されるユーザーエージェントをカスタマイズする方法も示しています。
<!-- The above example also demonstrates a way to customize the user agent seen by the remote web server. -->

## jQueryや他のライブラリの使用
<!-- ## Use jQuery and Other Libraries -->

バージョン1.6から、次のようにpage.includeJsを使用することで、ページにjQueryをインクルードすることができます:
<!-- As of version 1.6, you are also able to include jQuery into your page using a page.includeJs as follows: -->

```javascript
var page = require('webpage').create();
page.open('http://www.sample.com', function() {
    page.includeJs("http://ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js", function() {
        page.evaluate(function() {
            $("button").click();
        });
        phantom.exit()
    });
});
```

上記のスニペットはWebページを開き、ページにjQueryライブラリをインクルードした後、jQueryを使用してすべてのボタンをクリックします。そしてWebページを終了します。page.includeJs内にexit文を入れているか、またはjavascriptのコードが含まれる前に、途中で終了する可能性があることを確認してください。
<!-- The above snippet will open up a web page, include the jQuery library into the page, and then click on all buttons using jQuery. It will then exit from the web page. Make sure to put the exit statement within the page.includeJs or else it may exit prematurely before the javascript code is included. -->