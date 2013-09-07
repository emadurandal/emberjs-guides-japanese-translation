# REPLACING THE FIXTURE ADAPTER WITH ANOTHER ADAPTER
# フィクスチャアダプタを他のアダプタに置き換える

Finally we'll replace our fixture data with real persistence so todos will remain between application loads by replacing the fixture adapter with a `localstorage`-aware adapter instead.

最後に、フィスチャアダプタを`localstorage`を使うアダプタに置き換えることで、フィクスチャデータを実際の永続データに置き換え、アプリケーションを再起動してもTodoが保持されるようにします。

Change `js/application.js` to:

`js/application.js`を次のように変更してください。

```javascript
window.Todos = Ember.Application.create();

Todos.ApplicationAdapter = DS.LSAdapter.extend({
  namespace: 'todos-emberjs'
});
```

The local storage adapter, written by Ryan Florence, can be downloaded [from its source](https://github.com/rpflorence/ember-localstorage-adapter). Add it to your project as `js/libs/local_storage_adapter.js`. You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created the file and named it as indicated.

Ryan Florence氏によって書かれたローカルストレージアダプタはここからダウンロードできます。そのコードをあなたのプロジェクトの`js/libs/local_storage_adapter.js`に追加してください。あなたはこのファイルを好きな場所に配置することができます（全てのコードを同じファイルに入れてしまうこともできます）が、このガイドでは、あなたが指定された名前でファイルを作成したと想定します。

In `index.html` include `js/libs/local_storage_adapter.js` as a dependency:

`index.html`にて、依存ファイルとして`js/libs/local_storage_adapter.js`をインクルードしてください。

```html
<!--- ... additional lines truncated for brevity ... -->
<script src="js/libs/ember-data.js"></script>
<script src="js/libs/local_storage_adapter.js"></script>
<script src="js/application.js"></script>
 <!--- ... additional lines truncated for brevity ... -->
```

Reload your application. Todos you manage will now persist after the application has been closed.

アプリケーションをリロードしてください。アプリケーションを閉じた後でも、あなたが管理するTodoが維持されているはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/aZIXaYo/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/81801d87da42d0c83685ff946c46de68589ce38f)
  * [LocalStorage Adapter on GitHub](https://github.com/rpflorence/ember-localstorage-adapter)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)