# DISPLAYING A BUTTON TO REMOVE ALL COMPLETED TODOS
# すべての完了済みTodoを削除するボタンを表示する

TodoMVC allows users to delete all completed todos at once by clicking a button. This button is visible only when there are any completed todos, displays the number of completed todos, and removes all todos from the application when clicked.

TodoMVCでは、ユーザーはボタンをクリックすることで、一度にすべての完了済みTodoを削除することができます。このボタンは完了済みのTodoが存在するときにのみ表示され、完了済みTodoの数を表示し、クリックされるとアプリケーションから全ての完了済みTodoを削除します。

In this step, we'll implement that behavior. In `index.html` update the static `<button>` for clearing all completed todos:

このステップでは、その動作を実装します。`index.html`にて、すべての完了済みTodoをクリアするために、静的な`<button>`要素を更新します。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
{{#if hasCompleted}}
  <button id="clear-completed" {{action "clearCompleted"}}>
    Clear completed ({{completed}})
  </button>
{{/if}}
<!--- ... additional lines truncated for brevity ... -->
```

In `js/controllers/todos_controller.js` implement the matching properties and a method that will clear completed todos and persist these changes when the button is clicked:

`js/controllers/todos_controllers.js`にて、ボタンがクリックされたときに、完了済みのTodoをクリアして、この変更を保存するため、対応するプロパティとメソッドを実装します。

```javascript
// ... additional lines truncated for brevity ...
actions: {
  clearCompleted: function () {
    var completed = this.filterProperty('isCompleted', true);
    completed.invoke('deleteRecord');
    completed.invoke('save');
  }
},
hasCompleted: function () {
  return this.get('completed') > 0;
}.property('completed'),

completed: function () {
  return this.filterProperty('isCompleted', true).get('length');
}.property('@each.isCompleted')
// ... additional lines truncated for brevity ...
```

Reload your web browser to ensure that there are no errors and the behavior described above occurs. 

ウェブブラウザーをリロードして、何もエラーが起きないことと、上述の動作が行われることを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/ULovoJI/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/1da450a8d693f083873a086d0d21e031ee3c129e)
  * [Handlebars Conditionals Guide](/guides/templates/conditionals)
  * [Enumerables Guide](/guides/enumerables)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)