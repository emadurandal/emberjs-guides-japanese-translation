# INTEGRATION TESTS
# 統合テスト

Integration tests are generally used to test important workflows within your application. They emulate user interaction and confirm expected results.

一般的に、統合テストはあなたのアプリケーションの中の重要なワークフローをテストするのに用いられます。統合テストではユーザー操作をエミュレートし、期待された結果を確かめます。

### Setup

In order to integration test the Ember application, you need to run the app within your test framework. Set the root element of the application to an arbitrary element you know will exist. It is useful, as an aid to test-driven development, if the root element is visible while the tests run. You can potentially use #qunit-fixture, which is typically used to contain fixture html for use in tests, but you will need to override css to make it visible.

Emberアプリケーションを統合テストするために、あなたはアプリケーションをテストフレームワークの中で実行させる必要があります。アプリケーションのルート要素を、Emberアプリケーションを含んだあなたが知る任意の要素にしてください。もしテストを実行している間、そのルート要素が可視状態なら、テスト駆動開発を助ける意味において、そうすることが有用です。
あなたは、テストで使うフィクスチャーHTMLを含むために一般的に用いられる#qunit-fixtureを使うこともできます。しかし、あなたはそれを可視状態にするためにCSSを上書きする必要があるでしょう。

```javascript
App.rootElement = '#arbitrary-element-to-contain-ember-application';
```

This hook defers the readiness of the application, so that you can start the app when your tests are ready to run. It also sets the router's location to 'none', so that the window's location will not be modified (preventing both accidental leaking of state between tests and interference with your testing framework).

このフック（`App.setupForTesting()`）はアプリケーションの準備を引き延ばします。なので、あなたはテストの実行の準備が整ってから、アプリケーションをスタートさせることができます。また、このフックはRouterの場所を'none'にセットしているため、（ブラウザの）windowのlocationは修正されません（そのことで、テスト間の状態の予想外の漏洩とあなたのテスティングフレームワークとの衝突の両方を防止します。）。

```javascript
App.setupForTesting();
```

This injects the test helpers into the window's scope.

このコード（`App.injectTestHelpers()`）は、テストヘルパーをwindowのスコープに追加します。

```javascript
App.injectTestHelpers();
```

With QUnit, `setup` and `teardown` functions can be defined in each test module's configuration. These functions are called for each test in the module. If you are using a framework other than QUnit, use the hook that is called before each individual test.

QUnitでは、`setup`と`teardown`関数が、各テストモジュールの設定において定義できます。これらの関数はモジュールの各テストのために呼び出されます。もしあなたがQUnit以外のテストフレームワークを使っているなら、各個別のテストの前に呼ばれるフックを使ってください。

After each test, reset the application: `App.reset()` completely resets the state of the application.

各テストの後に、アプリケーションをリセットしてください。`App.reset()`はアプリケーションの状態を完全にリセットします。

```javascript
module("Integration Tests", {
  teardown: function() {
    App.reset();
  }
});
```

### Test adapters for other libraries
### 他のライブラリのためのテストアダプター

If you use a library other than QUnit, your test adapter will need to provide methods for `asyncStart` and `asyncEnd`. To facilitate asynchronous testing, the default test adapter for QUnit uses methods that QUnit provides: (globals) `stop()` and `start()`.

もしあなたがQUnit以外のライブラリを使っているなら、あなたのテストアダプターが`asyncStart`と`asyncEnd`のためのメソッドを提供していることが必要とされます。非同期のてテストを容易にするために、QUnitのデフォルトテストアダプターはQUnitが提供している（グローバルの）`stop()`と`start()`メソッドを使っています。

**Please note:**
**注意事項**

The `ember-testing` package is not included in the production builds, only development builds of Ember include the testing package. The package can be loaded in your dev or qa builds to facilitate testing your application. By not including the ember-testing package in production, your tests will not be executable in a production environment.

Emberの開発ビルドだけがテスティングパッケージを含んでいます。ember-testingパッケージはEmberのプロダクションビルドには含まれていません。このパッケージは、あなたのアプリケーションのテストを助けるために、あなたのアプリケーションの開発またはQAビルドにロードすることができます。プロダクションビルドにember-testingパッケージが含まれていないことから、あなたのテストはプロダクション環境では実行できません。

(The original document’s commit SHA1: 9e7e4a69b00ecfbfa98c7ac6fd534e567d841cc2)