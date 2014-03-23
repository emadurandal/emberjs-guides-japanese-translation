# ADDING CHILD ROUTES
# 子ルートの追加

Next we will split our single template into a set of nested templates so we can transition between different lists of todos in reaction to user interaction.

次に、単一のテンプレートをネストされたテンプレートのセットに分割します。そうすることで、ユーザーの操作に反応して、異なるTodoのリストの間を遷移することができるようになります。

In `index.html` move the entire `<ul>` of todos into a new template named `todos/index` by adding a new Handlebars template `<script>` tag inside the `<body>` of the document:

`index.html`にて、ドキュメントの`<body>`要素の中に新しいHandlebarsテンプレートの`<script>`タグを追加し、Todoの`<ul>`要素の全体を`todos/index`と名付けた新しいテンプレートに移動させます。

```html
<!--- ... additional lines truncated for brevity ... -->
<script type="text/x-handlebars" data-template-name="todos/index">
  <ul id="todo-list">
    {{#each itemController="todo"}}
      <li {{bind-attr class="isCompleted:completed isEditing:editing"}}>
        {{#if isEditing}}
          {{edit-todo class="edit" value=title focus-out="acceptChanges" insert-newline="acceptChanges"}}
        {{else}}
          {{input type="checkbox" checked=isCompleted class="toggle"}}
          <label {{action "editTodo" on="doubleClick"}}>{{title}}</label><button {{action "removeTodo"}} class="destroy"></button>
        {{/if}}
      </li>
    {{/each}}
  </ul>
</script>
<!--- ... additional lines truncated for brevity ... -->
```

Still within `index.html` place a Handlebars `{{outlet}}` helper where the `<ul>` was previously:

また`index.html`にて、それまで`<ul>`要素があった場所に、Handlebarsの`{{outlet}}`ヘルパーを配置してください。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<section id="main">
  {{outlet}}

  <input type="checkbox" id="toggle-all">
</section>
<!--- ... additional lines truncated for brevity ... -->
```

The `{{outlet}}` Handlebars helper designates an area of a template that will dynamically update as we transition between routes. Our first new child route will fill this area with the list of all todos in the application.

Handlebarsの`{{outlet}}`ヘルパーは、私たちがルート間を遷移するのに応じて、動的に更新されるテンプレートの領域を指定します。私たちの最初の子ルートは、このエリアに、アプリケーションのすべてのTodoリストを挿入します。

In `js/router.js` update the router to change the `todos` mapping, with an additional empty function parameter so it can accept child routes, and add this first `index` route:

`js/router.js`にて、`todos`のマッピングを変更するよう、追加の空の関数のパラメーターでRouterを更新します。そうすることで、Routerは子ルートを受け入れ、この最初の`index`Routeを追加できるようになります。

```javascript
Todos.Router.map(function () {
  this.resource('todos', { path: '/' }, function () {
    // additional child routes will go here later
  });
});

// ... additional lines truncated for brevity ...

Todos.TodosIndexRoute = Ember.Route.extend({
  model: function() {
    return this.modelFor('todos');
  }
});
```

When the application loads at the url `'/'` Ember.js will enter the `todos` route and render the `todos` template as before. It will also transition into the `todos.index` route and fill the `{{outlet}}` in the `todos` template with the `todos/index` template.  The model data for this template is the result of the `model` method of `TodosIndexRoute`, which indicates that the
model for this route is the same model as for the `TodosRoute`.

アプリケーションがURL `'/'`をロードしたとき、Ember.jsは`todos`Routeに入り、これまで通り、todosテンプレートをレンダリングします。Ember.jsは`todos.index`Routeにも遷移し、`todos`テンプレートの`{{outlet}}`に、`todos/index`テンプレートを挿入します。この`todos/index`テンプレートのためのモデルデータは`TodosIndexRoute`の`model`メソッドの戻り値です。このRouteのモデルは、`TodosRoute`のモデルと同じものです。

This mapping is described in more detail in the [Naming Conventions Guide](/guides/concepts/naming-conventions).

このマッピングについてのより詳しい詳細は、命名規則ガイドで説明しています。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/oweNovo/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/3bab8f1519ffc1ca2d5a12d1de35e4c764c91f05)
  * [Ember Router Guide](/guides/routing)
  * [Ember Controller Guide](/guides/controllers)
  * [outlet API documentation](/api/classes/Ember.Handlebars.helpers.html#method_outlet)

(The original document’s commit SHA1: d33fd8f5a54b5e6038e9af49bdbca33a612d82b0)