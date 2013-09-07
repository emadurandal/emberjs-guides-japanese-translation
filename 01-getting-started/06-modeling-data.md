# MODELING DATA
# データのモデリング

Next we will create a model class to describe todo items. 

次に、todo項目を表現するモデルクラスを作成します。

Create a file at `js/models/todo.js` and put the following code inside:

`js/models/todo.js`ファイルを作成し、中に次のコードを追加します。

```javascript
Todos.Todo = DS.Model.extend({
  title: DS.attr('string'),
  isCompleted: DS.attr('boolean')
});
```

This code creates a new class `Todo` and places it within your application's namespace. Each todo will have two attributes: `title` and `isCompleted`.

このコードは新しいクラスTodoを生成し、それをあなたのアプリケーションのネームスペースの中に配置します。各Todoは二つの属性、`title`と`isCompleted`を持ちます。

You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created a file and named it as indicated.

あなたはこれらのファイルをどこでも好きな場所に配置してよいですが（全てのコードを同一ファイルに押し込めてもかまいません）、このガイドでは、あなたが上述した指定の名前のファイルを作成することを想定します。

Finally, update your `index.html` to include a reference to this new file:

最後に、これらの新しいファイルをインクルードするようにindex.htmlを更新してください。

```html
<!-- ... additional lines truncated for brevity ... -->
  <script src="js/models/todo.js"></script>
</body>
<!-- ... additional lines truncated for brevity ... -->
```

Reload your web browser to ensure that all files have been referenced correctly and no errors occur.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/AJoyOGo/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/a1ccdb43df29d316a7729321764c00b8d850fcd1)
  * [Defining A Store Guide](/guides/models/defining-a-store)
  * [Defining Models Guide](/guides/models/defining-models)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)