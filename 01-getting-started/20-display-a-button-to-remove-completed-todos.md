# DISPLAYING A BUTTON TO REMOVE ALL COMPLETED TODOS
# すべての完了済みTodoを削除するボタンを表示する

TodoMVC allows users to delete all completed todos at once by clicking a button. This button is visible only when there are any completed todos, displays the number of completed todos, and removes all completed todos from the application when clicked.

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
  clearCompleted: function() {
    var completed = this.filterBy('isCompleted', true);
    completed.invoke('deleteRecord');
    completed.invoke('save');
  },
  // ... additional lines truncated for brevity ...
},
hasCompleted: function() {
  return this.get('completed') > 0;
}.property('completed'),

completed: function() {
  return this.filterBy('isCompleted', true).get('length');
}.property('@each.isCompleted'),
// ... additional lines truncated for brevity ...
```

The `completed` and `clearCompleted` methods both invoke the `filterBy` method, which is part of the [ArrayController](http://emberjs.com/api/classes/Ember.ArrayController.html#method_filterProperty) API and returns an instance of [EmberArray](http://emberjs.com/api/classes/Ember.Array.html) which contains only the items for which the callback returns true.  The `clearCompleted` method also invokes the `invoke` method which is part of the [EmberArray](http://emberjs.com/api/classes/Ember.Array.html#method_invoke) API.  `invoke` will execute a method on each object in the Array if the method exists on that object.

`completed`と`clearCompleted`メソッドはともに`filterBy`メソッドを呼び出します。この`filterBy`メソッドは[ArrayController](http://emberjs.com/api/classes/Ember.ArrayController.html#method_filterProperty) APIの一部で、コールバックがtrueを返す項目だけを含んだEmberArrayのインスタンスを返します。`clearCompleted`メソッドは、[EmberArray](http://emberjs.com/api/classes/Ember.Array.html#method_invoke) APIの一部である`invoke`メソッドも呼び出します。`invoke`は、その配列の各オブジェクトについて、（引数で与えられた）メソッドがオブジェクトに存在している場合に、そのメソッドを呼び出します。

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

(The original document’s commit SHA1: 0ca0cbe598f1f8ebd89b8850b26e39dfbd768e07)