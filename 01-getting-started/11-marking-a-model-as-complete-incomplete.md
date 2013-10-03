# MARKING A MODEL AS COMPLETE OR INCOMPLETE
# モデルを完了か未完了かにマークする

In this step we'll update our application to allow a user to mark a todo as complete or incomplete and persist the updated information.

このステップで私たちは、ユーザーがTodoを完了か未完了かにマークし、更新された情報を保存できるように、アプリケーションを更新します。

In `index.html` update your template to wrap each todo in its own controller by adding an `itemController` argument to the `{{each}}` Handlebars helper. Then convert our static `<input type="checkbox">` into a `{{input}}` helper:

`index.html`でテンプレートを更新して、Handlebarsの`{{each}}`ヘルパーに`itemController`属性を追加することで、各TodoをTodoごとに個別のControllerの中に包むようにしてください。そして静的な`<input type="checkbox">`を`{{input}}`ヘルパーに置き換えてください。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
{{#each itemController="todo"}}
  <li {{bind-attr class="isCompleted:completed"}}>
    {{input type="checkbox" checked=isCompleted class="toggle"}}
    <label>{{title}}</label><button class="destroy"></button>
  </li>
{{/each}}
<!--- ... additional lines truncated for brevity ... -->
```

When this `{{input}}` is rendered it will ask for the current value of the controller's `isCompleted` property. When a user clicks this input, it will call the controller's `isCompleted` property with an argument of either `true` or `false` depending on the new checked value of the input.

この`{{input}}`ヘルパーがレンダリングされるとき、`{{input}}`ヘルパーはControllerのisCompletedプロパティの現在の値を問い合わせます。ユーザーがこの入力をクリックしたとき、この入力（`{{input}}`ヘルパー）はチェックされた値に応じて、`true`か`false`のいずれかの引数とともにControllerの`isCompleted`プロパティを呼び出します。

Implement the controller for each todo by matching the name used as the `itemController` value to a class in your application `Todos.TodoController`. Create a new file at `js/controllers/todo_controller.js` for this code. You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created the file and named it as indicated.

`itemController`の値として使われている名前とあなたのアプリケーションの`Todos.TodoController`のクラスをマッチさせることで、各TodoのためのControllerを実装してください（翻訳者補足：つまり、itemControllerの値が”todo”だとしたら、実装すべきControllerの名前も”Todo”から始まる必要がある）。このコードのために新しいファイル`js/controllers/todo_controller.js`を作成します。あなたはこれらのファイルをどこでも好きな場所に配置してよいですが（全てのコードを同一ファイルの中に入れてもかまいません）、このガイドでは、あなたが上述した指定の名前のファイルを作成することを想定します。

Inside `js/controllers/todo_controller.js` add code for `Todos.TodoController` and its `isCompleted` property:

`js/controllers/todo_controller.js`に、`Todos.TodoController`とその`isCompleted`プロパティのコードを追加してください。

```javascript
Todos.TodoController = Ember.ObjectController.extend({
  isCompleted: function(key, value){
    var model = this.get('model');

    if (value === undefined) {
      // property being used as a getter
      return model.get('isCompleted');
    } else {
      // property being used as a setter
      model.set('isCompleted', value);
      model.save();
      return value;
    }
  }.property('model.isCompleted')
});
```

When called from the template to display the current `isCompleted` state of the todo, this property will proxy that question to its underlying `model`. When called with a value because a user has toggled the checkbox in the template, this property will set the `isCompleted` property of its `model` to the passed value (`true` or `false`), persist the model update, and return the passed value so the checkbox will display correctly.

Todoの現在の`isCompleted`状態を表示するためにテンプレートから呼び出されたとき、このプロパティはModelへの問い合わせをプロキシ（中継）します。ユーザーがテンプレートのチェックボックスをトグルしたことで（値を伴って）呼び出されたときは、このプロパティはModelの`isCompleted`プロパティを渡された値（`true`か`false`）にセットし、Modelの更新を保存し、チェックボックスが正しく表示できるよう、渡された値を返します。

In `index.html` include `js/controllers/todo_controller.js` as a dependency:

`index.html`で、依存ファイルとして`js/controllers/todo_controller.js`をインクルードします。

```html
<!--- ... additional lines truncated for brevity ... -->
   <script src="js/models/todo.js"></script>
   <script src="js/controllers/todos_controller.js"></script>
   <script src="js/controllers/todo_controller.js"></script>
 </body>
 <!--- ... additional lines truncated for brevity ... -->
```

 Reload your web browser to ensure that all files have been referenced correctly and no errors occur. You should now be able to change the `isCompleted` property of a todo.
 
ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。これで、あなたはTodoの`isCompleted`プロパティを変更できるはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/UDoPajA/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/8d469c04c237f39a58903a3856409a2592cc18a9)
  * [Ember.Checkbox API documentation](/api/classes/Ember.Checkbox.html)
  * [Ember Controller Guide](/guides/controllers)
  * [Computed Properties Guide](/guides/object-model/computed-properties/)
  * [Naming Conventions Guide](/guides/concepts/naming-conventions)

(The original document’s commit SHA1: 6bba45e2c8ebee89d85db44a3e969c9046401866)