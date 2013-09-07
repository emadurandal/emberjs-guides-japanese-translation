# Using Fixtures
# フィクスチャの使用

Now we'll add fixture data. Fixtures are a way to put sample data into an application before connecting the application to long-term persistence.

さて、フィクスチャデータを追加しましょう。フィクスチャは、アプリケーションを長期間の永続化に接続する前に、アプリケーションにサンプルデータを投入する方法です。

First, update `js/application.js` to indicate that your application's `ApplicationAdapter`
is an extension of the `DS.FixtureAdapter`. Adapters are responsible for communicating with a source of data for your application. Typically this will be a web service API, but in this case we are using an adapter designed to load fixture data:

最初に、`js/application.js`を更新して、あなたのアプリケーションの`ApplicationAdapter`が`DS.FixtureAdapter`の拡張であることを指示しましょう。アダプターはあなたのアプリケーションのための、データソースとの通信について責任を持ちます。通常は、これはWebサービスAPIになりますが、今回の場合では、フィクスチャデータを読み込むように設計されたアダプタを使います。

```javascript
window.Todos = Ember.Application.create();

Todos.ApplicationAdapter = DS.FixtureAdapter.extend();
```


Next, update the file at `js/models/todo.js` to include the following fixture data:
次に、`js/models/todo.js`を更新して、次のフィクスチャデータを含めましょう。

```javascript
// ... additional lines truncated for brevity ...
Todos.Todo.FIXTURES = [
 {
   id: 1,
   title: 'Learn Ember.js',
   isCompleted: true
 },
 {
   id: 2,
   title: '...',
   isCompleted: false
 },
 {
   id: 3,
   title: 'Profit!',
   isCompleted: false
 }
];
```

Reload your web browser to ensure that all files have been referenced correctly and no errors occur.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/Ovuw/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/a586fc9de92cad626ea816e9bb29445525678098)

(The original document’s commit SHA1: e828694c19e0d3a32ec92f50259d889580fde6a5)