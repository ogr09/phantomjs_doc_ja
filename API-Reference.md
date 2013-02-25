# APIリファレンス

_**これは随時更新していくドキュメントです。コー​​ドベースが更新された時、我々はこのドキュメントも同様に最新の状態を維持したいと考えています。特に断りのない限り、このドキュメントは現在最新のPhantomJSのリリースに適用されます:** PhantomJS 1.8.0_
<!-- _**This is a living document. As the codebase is updated, we hope to keep this document updated as well. Unless otherwise stated, this document currently applies to the latest PhantomJS release:** PhantomJS 1.8.0_ -->

**注:** このページはリファレンスとして役立ちます。PhantomJSの使い方をステップバイステップで学習するためには、[クイックスタートガイド](./Quick-Start.md)を参照してください。
<!-- **Note:** This page serves as a reference. To learn step-by-step on how to use PhantomJS, please refer to the [[Quick Start guide|Quick Start]]. -->

PhantomJSが[ビルド](http://phantomjs.org/build.html)されていて、その実行可能ファイルがどこかPATHの通った場所に置かれていると仮定すると、次のように呼び出すことができます:
<!-- Assuming PhantomJS is [built](http://phantomjs.org/build.html) and its executable is place somewhere in the PATH, it can be invoked as follows: -->
`phantomjs [options] somescript.js [arg1 [arg2 [...]]]`

空ページがWebブラウザで実行されているかのようにスクリプトコードが実行されます。PhantomJSはヘッドレスであるため、画面上に表示されるものがないでしょう。
<!-- The script code will be executed as if it running in a web browser with an empty page. Since PhantomJS is headless, there will not be anything visible shown up on the screen. -->

任意の引数を指定せずにPhantomJSを呼び出した場合は、対話モード（REPL）になります。
<!-- If PhantomJS is invoked without any argument, it will enter the interactive mode (REPL). -->

<a name="command-line-options" />
## コマンドラインオプション ##
<!-- ## Command-line Options ## -->
サポートされているコマンドラインオプションは以下のとおりです:
<!-- Supported command-line options are: -->
 * `--help` or `-h` 可能なすべてのコマンドラインオプションの一覧を表示します。 _すぐに停止し、引数として渡されたスクリプトを実行しません。_
 * `--version` or `-v` PhantomJSのバージョンを出力します。 _すぐに停止し、引数として渡されたスクリプトを実行しません。_
 * `--cookies-file=/path/to/cookies.txt` 永続的な[クッキー](#cookie)をストアするファイル名を指定します。
 * `--disk-cache=[true|false]` ディスクキャッシュを有効にします（場所はデスクトップサービスのキャッシュストレージで、デフォルトは`false`）。また次も可能です: `[yes|no]`。
 * `--ignore-ssl-errors=[true|false]` 期限切れまたは自己署名証明書エラーなどのSSLエラーを無視します（デフォルトは`false`）。また次も可能です: `[yes|no]`。
 * `--load-images=[true|false]` インライン化されたすべての画像を読み込みます（デフォルトは`true`）。また次も可能です: `[yes|no]`。
 * `--local-storage-path=/some/path` ローカルストレージのコンテンツやWebSQLコンテンツを保存するパス。
 * `--local-storage-quota=number` 最大データサイズ。
 * `--local-to-remote-url-access=[true|false]` ローカルコンテンツがリモートURLにアクセスできるようにします（デフォルトは`false`）。また次も可能です: `[yes|no]`。
 * `--max-disk-cache-size=size` ディスクキャッシュのサイズを制限します（KB単位）。
 * `--output-encoding=encoding` 端末出力で使用されるエンコードを設定します（デフォルトは`utf8`）。
 * `--proxy=address:port` 使用するプロキシサーバーを指定します（例： `--proxy=192.168.1.42:8080`）。
 * `--proxy-type=[http|socks5|none]` プロキシサーバーの種類を指定します（デフォルトは`http`）。
 * `--script-encoding=encoding` 開始スクリプトのエンコードを設定します（デフォルトは`utf8`）。
 * `--ssl-protocol=[sslv3|sslv2|tlsv1|any']` 保護された接続用のSSLプロトコルを設定します（デフォルトは`SSLv3`）。
 * `--web-security=[true|false]` Webセキュリティを有効にし、クロスドメインXHRを禁止します（デフォルトは`true`）。また次も可能です: `[yes|no]`。

<!--
 * `--help` or `-h` lists all possible command-line options. _Halts immediately, will not run a script passed as argument._
 * `--version` or `-v` prints out the version of PhantomJS. _Halts immediately, will not run a script passed as argument._
 * `--cookies-file=/path/to/cookies.txt` specifies the file name to store the persistent [Cookies](#cookie).
 * `--disk-cache=[true|false]` enables disk cache (at desktop services cache storage location, default is `false`). Also accepted: `[yes|no]`.
 * `--ignore-ssl-errors=[true|false]` ignores SSL errors, such as expired or self-signed certificate errors (default is `false`). Also accepted: `[yes|no]`.
 * `--load-images=[true|false]` load all inlined images (default is `true`). Also accepted: `[yes|no]`.
 * `--local-storage-path=/some/path` path to save LocalStorage content and WebSQL content.
 * `--local-storage-quota=number` maximum size to allow for data.
 * `--local-to-remote-url-access=[true|false]` allows local content to access remote URL (default is `false`). Also accepted: `[yes|no]`.
 * `--max-disk-cache-size=size` limits the size of disk cache (in KB).
 * `--output-encoding=encoding` sets the encoding used for terminal output (default is `utf8`).
 * `--proxy=address:port` specifies the proxy server to use (e.g. `--proxy=192.168.1.42:8080`).
 * `--proxy-type=[http|socks5|none]` specifies the type of the proxy server (default is `http`).
 * `--script-encoding=encoding` sets the encoding used for the starting script (default is `utf8`).
 * `--ssl-protocol=[sslv3|sslv2|tlsv1|any']` sets the SSL protocol for secure connections (default is `SSLv3`).
 * `--web-security=[true|false]` enables web security and forbids cross-domain XHR (default is `true`). Also accepted: `[yes|no]`.
-->

またPhantomJS 1.3から、複数のコマンドラインオプションを渡す代わりに、JavaScript Object Notation（JSON）の設定ファイルを利用することもできます:
<!-- Alternatively, since PhantomJS 1.3, you can also utilize a JavaScript Object Notation (JSON) configuration file instead of passing in multiple command-line options: -->
 * `--config=/path/to/config.json`

config.jsonの内容は、スタンドアロンのJavaScriptオブジェクトである必要があります。Keys are de-dashed, camel-cased equivalents of the other supported command-line options (excluding `--version`/`-v` and `--help`/`-h`). 値はJavaScriptと等価です:'true'/'false'（または 'yes'/'no'）はブール値の`true`/`false`に変換され、数値は数値のまま、文字列は文字列のままになります。例:
<!-- The contents of `config.json` should be a standalone JavaScript object. Keys are de-dashed, camel-cased equivalents of the other supported command-line options (excluding `--version`/`-v` and `--help`/`-h`).  Values are their JavaScript equivalents: 'true'/'false' (or 'yes'/'no') values translate into `true`/`false` Boolean values, numbers remain numbers, strings remain strings. For example: -->
```js
{
    /* Same as: --ignore-ssl-errors=true */
    "ignoreSslErrors": true,

    /* Same as: --max-disk-cache-size=1000 */
    "maxDiskCacheSize": 1000,

    /* Same as: --output-encoding=utf8 */
    "outputEncoding": "utf8"

    /* etc. */
}
```

<a name="phantom" />
## オブジェクト: `phantom` ##
<!-- ## Object: `phantom` ## -->
[専用ページを参照してください](API-Reference-phantom.md)
<!-- [[See dedicated page |API-Reference-phantom]] -->



<a name="module-api" />
# モジュールAPI #
<!-- # Module API # -->
[CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1.1)が利用できる後に、モジュールのAPIがモデル化されます。PhantomJS 1.6から、組み込まれたモジュールのみがサポートされています:
<!-- The Module API is modeled after [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.1.1) is available. Up through PhantomJS 1.6, the only supported modules that were built in: -->
 * [webpage](./API-Reference-WebPage.md)
 * [system](./API-Reference-System.md)
 * [fs](./API-Reference-FileSystem.md)
 * [webserver](./API-Reference-WebServer.md)

しかしPhantomJS 1.7で、ユーザーは[`require`](#require)を使用してファイルシステムから独自のモジュールを参照することもできます。
<!-- As of PhantomJS 1.7, however, users can reference their own modules from the file system using [`require`](#require) as well. -->

<a name="require" />
## 関数: `require` ##
<!-- ## Function: `require` ## -->
モジュールAPIをサポートするために、[CommonJS Modules' Require](http://wiki.commonjs.org/wiki/Modules/1.1.1#Require)がグローバルに使用できる後に、`require`関数はモデル化されます。一般的な使用方法:
<!-- To support the Module API, a `require` function modeled after [CommonJS Modules' Require](http://wiki.commonjs.org/wiki/Modules/1.1.1#Require) is globally available. General usage: -->
```js
var server = require('webserver').create();
var Awesome = require('MyAwesomeModule');
Awesome.do();
```

# 付録 #
<!-- # Appendix # -->
<a name="cookie" />
## `Cookie` {Object} ##
[`WebPage`](#webpage)のインスタンスと同様に、[`phantom`](#phantom)オブジェクトのさまざまなメソッドは、`Cookie`オブジェクトを利用します。オブジェクトリテラルを介してこれらを作成することがベストな方法です。
<!-- Various methods in the [`phantom`](#phantom) object, as well as in [`WebPage`](#webpage) instances, utilize `Cookie` objects. These are best created via object literals. -->

例:
<!-- For example: -->
```js
phantom.addCookie({
    'name':     'Valid-Cookie-Name',   /* required property */
    'value':    'Valid-Cookie-Value',  /* required property */
    'domain':   'localhost',           /* required property */
    'path':     '/foo',
    'httponly': true,
    'secure':   false,
    'expires':  (new Date()).getTime() + 3600   /* <- expires in 1 hour */
});
```