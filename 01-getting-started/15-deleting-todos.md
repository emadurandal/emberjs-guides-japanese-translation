# DELETING A MODEL
# モデルの削除

TodoMVC displays a button for removing todos next to each todo when its `<li>` is hovered. Clicking this button will remove the todo and update the display of remaining incomplete todos and remaining completed todos appropriately.

TodoMVCは、Todoの`<li>`要素がマウスホバーされたときに、Todoを削除するためのボタンをTodoの隣に表示します。このボタンをクリックすると、Todoが取り除かれ、未完了のTodoの残数と完了済みのTodoの残数の表示が適切に更新されます。

In `index.html` update the static `<button>` element to include an `{{action}}` Handlebars helper:

`index.html`で、静的な`<button>`要素を、Handlebarsの`{{action}}`ヘルパーを含むように更新します。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<button {{action "removeTodo"}} class="destroy"></button>
<!--- ... additional lines truncated for brevity ... -->
```

In `js/controllers/todo_controller.js` implement the `removeTodo` method referenced in the template's `{{action}}` Handlebars helper:

`js/controllers/todo_controller.js`にて、テンプレートの`{{action}}`Handelbarsヘルパーで参照されている`removeTodo`メソッドを実装します。

```javascript
// ... additional lines truncated for brevity ...
actions: {
  removeTodo: function () {
    var todo = this.get('model');
    todo.deleteRecord();
    todo.save();
  },
}
// ... additional lines truncated for brevity ...
```

This method will delete the todo locally and then persist this data change.

このメソッドはTodoを削除し、そしてこのデータ変更（Todo削除）の結果を保存します。

Because the todo is no longer part of the collection of all todos, its `<li>` element in the page will be automatically removed for us. If the deleted todo was incomplete, the count of remaining todos will be decreased by one and the display of this number will be automatically re-rendered. If the new count results in an inflection change between "item" and "items" this area of the page will be automatically re-rendered.

このTodoはもはやすべてのTodoのコレクションの一部ではないため、ページ中のこのTodoの`<li>`要素は自動的に取り除かれます。もし削除されたTodoが未完了状態だった場合、Todo残数のカウントが１つ少なくなり、この残数の表示は自動的に再レンダリングされます。もし新しいカウントで’item’の語尾変化（”item”か”items”か）が変わるようであれば、ページのそのエリアが自動的に再レンダリングされます。

Reload your web browser to ensure that there are no errors and the behaviors described above occurs. 

ウェブブラウザをリロードして、エラーが何もないことと、上述の動作が行われることを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/eREkanA/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/14e1f129f76bae8f8ea6a73de1e24d810678a8fe)
  * [action API documention](/api/classes/Ember.Handlebars.helpers.html#method_action)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)
