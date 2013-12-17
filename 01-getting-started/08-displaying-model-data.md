# DISPLAYING MODEL DATA
# モデルデータの表示

Next we'll update our application to display dynamic todos, replacing our hard coded section in the `todos` template.

次に、アプリケーションを動的なTodoを表示するように更新し、`todos`テンプレートのハードコードされたセクションを置き換えます。

Inside the file `js/router.js` implement a `TodosRoute` class with a `model` function that returns all the existing todos:

`js/router.js`ファイルの中で、全ての既存のTodoを返すmodel関数を持つ、`TodosRoute`クラスを実装します。

```javascript
// ... additional lines truncated for brevity ...
Todos.TodosRoute = Ember.Route.extend({
  model: function () {
    return this.store.find('todo');
  }
});
```

Because we hadn't implemented this class before, Ember.js provided a `Route` for us with the default behavior of rendering a matching template named `todos` using its [naming conventions for object creation](/guides/concepts/naming-conventions/).

私たちはそれまで、このクラスを実装していなかったので、Ember.jsは『オブジェクト生成のための命名規則』を使って、マッチするテンプレート（今回は、`todos`と名付けられたテンプレート）をレンダリングするというデフォルト動作を持つRouteを私たちのために提供していたのです。

Now that we need custom behavior (returning a specific set of models), we implement the class and add the desired behavior.

今では、私たちはカスタムの動作（モデルの特定のセットを返す）を必要としているので、私たちはクラスを実装し、望む動作を追加します。

Update `index.html` to replace the static `<li>` elements with a Handlebars `{{each}}` helper and a dynamic `{{title}}` for each item.

`index.html`を更新して、静的な`<li>`要素を、Handlebarsの`{{each}}`ヘルパーと各項目のための動的な`{{titile}}`で置き換えます。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<ul id="todo-list">
  {{#each}}
    <li>
      <input type="checkbox" class="toggle">
      <label>{{title}}</label><button class="destroy"></button>
    </li>
  {{/each}}
</ul>
<!--- ... additional lines truncated for brevity ... -->
```

The template loops over the content of its controller. This controller is an instance of `ArrayController` that Ember.js has provided for us as the container for our models. Because we don't need custom behavior for this object yet, we can use the default object provided by the framework.

このテンプレートは（このテンプレートに対応する）Controllerの内容でループ処理を行います。このControllerは、Ember.jsが私たちのモデルのためのコンテナとして提供した`ArrayController`のインスタンスです。私たちはこのオブジェクトについてカスタムの動作をまだ必要としていないので、フレームワークから提供されるデフォルトのオブジェクトを使用することができます。

Reload your web browser to ensure that all files have been referenced correctly and no errors occur.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/EJISAne/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>
  
### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/87bd57700110d9dd0b351c4d4855edf90baac3a8)
  * [Templates Guide](/guides/templates/handlebars-basics)
  * [Controllers Guide](/guides/controllers)
  * [Naming Conventions Guide](/guides/concepts/naming-conventions)

(The original document’s commit SHA1: 6f9572cfc2e63e04151c4401f9090825990580de)