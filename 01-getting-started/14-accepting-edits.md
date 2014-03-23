# ACCEPTING EDITS
# 編集結果を受け入れる

In the previous step we updated TodoMVC to allow a user to toggle the display of a text `<input>` for editing a todo's title. Next, we'll add the behavior that immediately focuses the `<input>` when it appears, accepts user input and, when the user presses the `<enter>` key or moves focus away from the editing `<input>` element, persists these changes, then redisplays the todo with its newly updated text.

前回のステップで、私たちは、ユーザーが`<input>`要素の表示・非表示をトグルしてTodoのタイトルを編集できるよう、TodoMVCを更新しました。次に、私たちは、`<input>`要素が現れたらすぐにそこにフォーカスし、ユーザーの入力を受け入れ、そしてユーザーが`<enter>`キーを押すか、フォーカスを編集中の`<input>`要素から移動させるかしたときに、変更を保存して、新しく更新されたテキストでTodoを再表示する、という動作を追加します。

To accomplish this, we'll create a new custom component and register it with Handlebars to make it available to our templates.

これを実現するために、新しいカスタムComponentを作成し、それをテンプレートで利用できるよう、Handlebarsに登録します。

Create a new file `js/views/edit_todo_view.js`. You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created the file and named it as indicated.

新しいファイル`js/views/edit_todo_view.js`を作成してください。あなたはこれらのファイルをどこでも好きな場所に配置してよいですが（全てのコードを同一ファイルの中に入れてもかまいません）、このガイドでは、あなたがこのガイドで指定した名前のファイルを作成したと想定します。

In `js/views/edit_todo_view.js` create an extension of `Ember.TextField`:

`js/views/edit_todo_view.js`で、Ember.TextFieldの拡張版を作成します。

```javascript
Todos.EditTodoView = Ember.TextField.extend({
  didInsertElement: function() {
    this.$().focus();
  }
});

Ember.Handlebars.helper('edit-todo', Todos.EditTodoView);
```

In `index.html` require this new file:

`index.html`はこの新しいファイルを必要とします。

```html
<!--- ... additional lines truncated for brevity ... -->
  <script src="js/controllers/todo_controller.js"></script>
  <script src="js/views/edit_todo_view.js"></script>
</body>
<!--- ... additional lines truncated for brevity ... -->
```

In `index.html` replace the static `<input>` element with our custom `{{edit-todo}}` component, connecting the `value` property, and actions:

`index.html`で、静的な`<input>`要素を私たちのカスタム`{{edit-todo}}` Componentで置き換えます。このコンポーネントは`value`プロパティと次のアクションに接続します。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
{{#if isEditing}}
  {{edit-todo class="edit" value=title focus-out="acceptChanges"
                           insert-newline="acceptChanges"}}
{{else}}
<!--- ... additional lines truncated for brevity ... -->
```

Pressing the `<enter>` key  will trigger the `acceptChanges` event on the instance of `TodoController`. Moving focus away from the `<input>` will trigger the `focus-out` event, calling a method `acceptChanges` on this view's instance of `TodoController`.

`<enter>`キーを押すと、`TodoController`のインスタンスにある`acceptChanges`イベントがトリガされます。`<input>`要素からフォーカスが外れると、`focus-out`イベントがトリガされ、このイベントはビューのTodoControllerインスタンスにある`acceptChanges`メソッドを呼び出します。

Additionally, we connect the `value` property of this `<input>` to the `title` property of this instance of `TodoController`. We will not implement a `title` property on the controller so it will retain the default behavior of [proxying all requests](http://emberjs.com/guides/controllers/#toc_representing-models) to its `model`.

さらに、この`<input>`要素の`value`プロパティを`TodoController`インスタンスの`title`プロパティに接続します。私たちはController上に`title`プロパティを実装していないので、Controllerはすべての要求をモデルにプロキシ（中継）するという標準の動作をとります。

A CSS class `edit` is applied for styling.

CSSクラス `edit` がスタイリングのために適用されます。

In `js/controllers/todo_controller.js`, add the method `acceptChanges` that we called from `EditTodoView`:

`js/controllers/todo_controller.js`に、`EditTodoView`から呼ばれる`acceptChanges`メソッドを追加します。

```javascript
// ... additional lines truncated for brevity ...
actions: {
  editTodo: function() {
    this.set('isEditing', true);
  },
  acceptChanges: function() {
    this.set('isEditing', false);

    if (Ember.isEmpty(this.get('model.title'))) {
      this.send('removeTodo');
    } else {
      this.get('model').save();
    }
  }
},
// ... additional lines truncated for brevity ...
```

This method will set the controller's `isEditing` property to false and commit all changes made to the todo.

このメソッドはコントローラーの`isEditing`プロパティをfalseにセットし、Todoに行われた全ての変更をコミットします。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/USOlAna/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/a7e2f40da4d75342358acdfcbda7a05ccc90f348)
  * [Controller Guide](/guides/controllers)
  * [Ember.TextField API documentation](/api/classes/Ember.TextField.html)

(The original document’s commit SHA1: 81c1d68309cf93b4d3944e38feef7d7254f72626)