# TOGGLING BETWEEN SHOWING AND EDITING STATES
# 表示中と編集中の状態の切替え

TodoMVC allows users to double click each todo to display a text `<input>` element where the todo's title can be updated. Additionally the `<li>` element for each todo obtains the CSS class `editing` for style and positioning.

TodoMVCではユーザーが各Todoをダブルクリックするとテキスト`<input>`要素を表示し、そこでTodoのタイトルを更新することができます。加えて、そのとき、各Todoの`<li>`要素はスタイルと位置のためのCSSクラス `editing` を得ます。

We'll update the application to allow users to toggle into this editing state for a todo. In `index.html` update the contents of the `{{each}}` Handlebars helper to:

ユーザーがTodoの状態を「編集中」に切り替えられるよう、アプリケーションを更新します。`index.html`の`{{each}}`Handlebarsヘルパーの内容を、次のように更新してください。

```handlebars
 <!--- ... additional lines truncated for brevity ... -->
{{#each itemController="todo"}}
  <li {{bind-attr class="isCompleted:completed isEditing:editing"}}>
    {{#if isEditing}}
      <input class="edit">
    {{else}}
      {{input type="checkbox" checked=isCompleted class="toggle"}}
      <label {{action "editTodo" on="doubleClick"}}>{{title}}</label><button class="destroy"></button>
    {{/if}}
  </li>
{{/each}}
 <!--- ... additional lines truncated for brevity ... -->
```

The above code applies three new behaviors to our application: it applies the CSS class `editing` when the controller's `isEditing` property is true and removes it when the `isEditing` property is false. We add a new `{{action}}` helper to the `<label>` so double-clicks will call `editTodo` on 
this todo's controller. Finally, we wrap our todo in a Handlebars `{{if}}` helper so a text `<input>` will display when we are editing and the todos title will display when we are not editing.

上のコードは、三つの新しい動作をアプリケーションに適用します。Controllerの`isEditing`プロパティがtrueの時は、CSSクラス `editing` を付加し、`isEditing`プロパティがfalseの時はCSSクラス `editing` を取り除きます。また、`<label>`要素に`{{action}}`ヘルパーを加えることで、`<label>`要素をダブルクリックすると、TodoのControllerの`editTodo`アクションが呼び出されるようにします。最後に、TodoをHandlebarsの`{{if}}`ヘルパーで囲み、編集中にはテキスト`<input>`要素が表示され、編集中でなければTodoのタイトルが表示されるようにします。

Inside `js/controllers/todo_controller.js` we'll implement the matching logic for this template behavior:

`js/controllers/todo_controller.js`で、このテンプレートの動作に対応するロジックを実装します。

```javascript
// ... additional lines truncated for brevity ...
actions: {
   editTodo: function() {
     this.set('isEditing', true);
   }
 },
 
isEditing: false,

// ... additional lines truncated for brevity ...
```

Above we defined an initial `isEditing` value of `false` for controllers of this type and said that when the `editTodo` action is called it should set the `isEditing` property of this controller to `true`.  This will automatically trigger the sections of template that use `isEditing` to update their rendered content.

上のコードでは、Controllerに初期値が`false`の`isEdithing`プロパティを定義し、`editTodo`アクションがコールされたら、isEditingプロパティを`true`にセットするようにしました。Controllerは、テンプレートのこのセクションを自動的にトリガし、`isEditing`プロパティを使ってレンダリングされる内容を更新します。

Reload your web browser to ensure that no errors occur. You can now double-click a todo to edit it.

ウェブブラウザをリロードして、エラーが何も起きていないことを確認してください。ここまでで、Todoをダブルクリックして編集状態にすることができるはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/usiXemu/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>
  
### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/616bc4f22900bbaa2bf9bdb8de53ba41209d8cc0)
  * [Handlebars Conditionals Guide](/guides/templates/conditionals)
  * [bind-attr API documentation](/api/classes/Ember.Handlebars.helpers.html#method_bind-attr)
  * [action API documentation](/api/classes/Ember.Handlebars.helpers.html#method_action)
  * [bind and bindAttr article by Peter Wagenet](http://www.emberist.com/2012/04/06/bind-and-bindattr.html)

(The original document’s commit SHA1: 7c1fc2d6b2bbff8f26af5691b05917ba8826cc64)