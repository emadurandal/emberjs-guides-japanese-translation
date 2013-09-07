# TRANSITIONING BACK TO SHOW ALL TODOS
# すべてのTodoの表示に戻る

Next we can update the application to allow navigating back to the list of all todos. 

次に、すべてのTodoリストの表示に戻れるように、アプリケーションを更新します。

In `index.html` convert the `<a>` tag for 'All' todos into a Handlebars `{{link-to}}` helper:

`index.html`にて、’All’なTodoのための`<a>`タグを、Handlebarsの`{{link-to}}`ヘルパーに置き換えます。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<li>
  {{#link-to "todos.index" activeClass="selected"}}All{{/link-to}}
</li>
<li>
  {{#link-to "todos.active" activeClass="selected"}}Active{{/link-to}}
</li>
<li>
  {{#link-to "todos.completed" activeClass="selected"}}Completed{{/link-to}}
</li>
<!--- ... additional lines truncated for brevity ... -->
```

Reload your web browser to ensure that there are no errors. You should be able to navigate between urls for all, active, and completed todos.

ウェブブラウザーをリロードして、何もエラーが起きないことを確認してください。ここまでで、すべてのTodo、アクティブなTodo、そして完了済みのTodoに対応するURLの間を移動できるはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/uYuGA/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/843ff914873081560e4ba97df0237b8595b6ae51)
  * [link-to API documentation](/api/classes/Ember.Handlebars.helpers.html#method_link-to)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)