# DISPLAYING THE NUMBER OF INCOMPLETE TODOS
# 未完了Todoの数の表示

Next we'll update our template's hard-coded count of completed todos to reflect the actual number of completed todos. Update `index.html` to use two properties:

次に、テンプレートのハードコードされた完了済みTodoのカウントを、実際の完了済みTodoの数を反映するように、更新しましょう。`index.html`を、つぎの二つのプロパティを使うように更新します。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<span id="todo-count">
  <strong>{{remaining}}</strong> {{inflection}} left
</span>
<!--- ... additional lines truncated for brevity ... -->
```

Implement these properties as part of this template's controller, the `Todos.TodosController`:

これらのプロパティを、このテンプレートのコントローラー、`Todos.TodosController`の一部として実装します。

```javascript
// Hint: these lines MUST NOT go into the 'actions' object.

// ... additional lines truncated for brevity ...
remaining: function () {
  return this.filterBy('isCompleted', false).get('length');
}.property('@each.isCompleted'),

inflection: function () {
  var remaining = this.get('remaining');
  return remaining === 1 ? 'item' : 'items';
}.property('remaining'),
// ... additional lines truncated for brevity ...
```

The `remaining` property will return the number of todos whose `isCompleted` property is false. If the `isCompleted` value of any todo changes, this property will be recomputed. If the value has changed, the section of the template displaying the count will be automatically updated to reflect the new value.

`remaining`プロパティは`isCompleted`プロパティがfalseであるTodoの数を返します。もしTodoの`isCompleted`の値が変化したら、この`remaining`プロパティは再計算されます。もし値が変化したら、テンプレートのカウントを表示しているセクションは、その新しい値を反映するために自動的に更新されます。

The `inflection` property will return either a plural or singular version of the word "item" depending on how many todos are currently in the list. The section of the template displaying the count will be automatically updated to reflect the new value.

`inflection`プロパティは、リストに現在いくつのTodoがあるかに応じて、"item"という単語の、複数形または単数形のいずれかのバージョンを返します。その文字を表示しているテンプレートのセクションは、新しい値を反映するために自動的に更新されます。

 Reload your web browser to ensure that no errors occur. You should now see an accurate number for remaining todos.
 
ウェブブラウザーをリロードして、エラーが何も起きていないことを確認してください。ここまでで、あなたは正確なTodoの残数を確認することができるはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/onOCIrA/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>
  
### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/b418407ed9666714c82d894d6b70f785674f7a45)
  * [Computed Properties Guide](/guides/object-model/computed-properties/) 

(The original document’s commit SHA1: 5e5ea4d2be986900d00ecba267feda82853d5d5b)