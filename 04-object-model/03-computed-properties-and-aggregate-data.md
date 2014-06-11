## COMPUTED PROPERTIES AND AGGREGATE DATA WITH @EACH
## Computed Propertiesと@eachによる総計値

Often, you may have a computed property that relies on all of the items in an
array to determine its value. For example, you may want to count all of the
todo items in a controller to determine how many of them are completed.

しばしば、あなたは、その値を決定するために、配列の全ての項目に依存したComputed Propertyを持っているかもしれません。例えば、Todoのうちいくつが完了済みになっているか決定するために、ControllerのTodo項目の全てをカウントしたいとしましょう。

Here's what that computed property might look like:

そのようなComputed Propertyは次のような形になります。

```javascript
App.TodosController = Ember.Controller.extend({
  todos: [
    Ember.Object.create({ isDone: false })
  ],

  remaining: function() {
    var todos = this.get('todos');
    return todos.filterBy('isDone', false).get('length');
  }.property('todos.@each.isDone')
});
```

Note here that the dependent key (`todos.@each.isDone`) contains the special
key `@each`. This instructs Ember.js to update bindings and fire observers for
this computed property when one of the following four events occurs:

依存しているキー（`todos.@each.isDone`）は特別なキーである`@each`を含んでいることに注意してください。これは、次の４つのイベントのうちのひとつが発生したときに、このComputed propertyのためのバインディングを更新し、オブザーバーを起動するように、Ember.jsに指示します。

1. The `isDone` property of any of the objects in the `todos` array changes.
`todos`配列のいずれかのオブジェクトのisDoneプロパティが変化する。
2. An item is added to the `todos` array.
`todos`配列に項目が追加された。
3. An item is removed from the `todos` array.
`todos`配列から項目が削除された。
4. The `todos` property of the controller is changed to a different array.
Controllerの`todos`プロパティが他の配列に変えられた。

In the example above, the `remaining` count is `1`:

上の例では、`remaing`のカウントは`1`になります。

```javascript
App.todosController = App.TodosController.create();
App.todosController.get('remaining');
// 1
```

If we change the todo's `isDone` property, the `remaining` property is updated
automatically:

もしtodoの`isDone`プロパティを変更したら、`remaining`プロパティは自動的に更新されます。

```javascript
var todos = App.todosController.get('todos');
var todo = todos.objectAt(0);
todo.set('isDone', true);

App.todosController.get('remaining');
// 0

todo = Ember.Object.create({ isDone: false });
todos.pushObject(todo);

App.todosController.get('remaining');
// 1
```

Note that `@each` only works one level deep. You cannot use nested forms like
`todos.@each.owner.name` or `todos.@each.owner.@each.name`.

`@each`は１階層の深さしか機能しないことに注意してください。`todos.@each.owner.name` や `todos.@each.owner.@each.name` のように、ネストされた形式は使えません。

(The original document’s commit SHA1: 867638002f7dc651eeac736266241e3d2af33aa9)