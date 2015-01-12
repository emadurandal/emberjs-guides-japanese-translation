# SENDING ACTIONS FROM COMPONENTS TO YOUR APPLICATION
# ComponentからアプリケーションにActionを送る

When a component is used inside a template, it has the ability to send
actions to that template's controller and routes. These allow the
component to inform the application when important events, such as the
user clicking a particular element in a component, occur.

Componentがテンプレートの中で使用されているとき、そのComponentはテンプレートのControllerとRouteにActionを送る能力を持ちます。このことにより、ユーザーがComponentの特定の要素をクリックするなどの、重要なイベントが起きたとき、Componentはアプリケーションにそのことを伝えることができます。

Like the `{{action}}` Handlebars helper, actions sent from components
first go to the template's controller. If the controller does not
implement a handler for that action, it will bubble to the template's
route, and then up the route hierarchy. For more information about this
bubbling behavior, see [Action
Bubbling](http://emberjs.com/guides/templates/actions/#toc_action-bubbling).

`{{action}}` Handlebarsヘルパーのように、Componentから送られたActionは最初にテンプレートのControllerに向かいます。もしそのControllerがそのActionのためのハンドラーを実装していなければ、ActionはテンプレートのRouteへ伝播し、Routeの階層を上っていきます。この伝播の動作のより詳細については、[Action Bubling (Actionの伝播)](http://emberjs.com/guides/templates/actions/#toc_action-bubbling)を参照してください。

Components are designed to be reusable across different parts of your
application. In order to achieve this reusability, it's important that
the actions that your components send can be specified when the component
is used in a template.

Componentはあなたのアプリケーションの異なる部分をまたがって再利用できるように設計されています。この再利用性を実現するために、そのComponentがテンプレートで使われるとき、Componentが送ったActionを特定できるようにすることが重要です。

In other words, if you were writing a button component, you would not
want to send a `click` action, because it is ambiguous and likely to
conflict with other components on the page. Instead, you would want to
allow the person using the component to specify which action to send
when the button was clicked.

言い換えれば、もしあなたがボタンのComponentを書いていたなら、`click` Actionは送りたいと思わないでしょう。なぜなら、それは曖昧かつページにある他のComponentと競合するかもしれないからです。代わりに、あなたは、Componentを使う人が、ボタンがクリックされたときにどのアクションを送るかを指定できるようしようと思うでしょう。

Luckily, components have a `sendAction()` method that allows them to
send actions specified when the component is used in a template.

幸運なことに、Componentには、テンプレートでComponentが使われるときに、指定されたActionを送れるようにする`sendAction()`メソッドがあります。

### Sending a Primary Action
### プライマリActionを送る

Many components only send one kind of action. For example, a button
component might send an action when it is clicked on; this is the
_primary action_.

多くのComponentはただ一種類のActionを送ります。例えば、ボタンComponentは、それがクリックされたときにActionを送るかもしれません。これは _プライマリAction_ です

To set a component's primary action, set its `action` attribute in
Handlebars:

CompomentのプライマリActionを設定するには、HandlebarsでComponentの`action`属性を設定します。

```handlebars
{{my-button action="showUser"}}
```

This tells the `my-button` component that it should send the `showUser`
action when it triggers its primary action.

この記述は`my-button`Componentに、ComponentがそのプライマリActionをトリガーした時に、`showUser`Actionを送るべきであることを伝えます。

So how do you trigger sending a component's primary action? After
the relevant event occurs, you can call the `sendAction()` method
without arguments:

では、あなたはどうやって送信中のComponentのプライマリActionをトリガするのでしょうか？　関係のあるイベントが起きたあとで、あなたは引数なしで`sendAction()`メソッドを呼ぶことができます。

```js
App.MyButtonComponent = Ember.Component.extend({
  click: function() {
    this.sendAction();
  }
});
```

In the above example, the `my-button` component will send the `showUser`
action when the component is clicked.

上の例では、`my-button` Componentはクリックされたときに、`showUser`Actionを送信します。

### Sending Parameters with an Action
### Actionとともにパラメーターを送る

You may want to provide additional context to the route or controller
handling an action. For example, a button component may want to tell a
controller not only that _an_ item was deleted, but also _which_ item.

Actionを扱うRouteやControllerに追加のコンテキストを供給したいと思うかもしれません。例えば、ボタンComponentはControllerに、項目がただ削除されたことだけでなく、_どの項目が_ 削除されたかも伝えたいかもしれません。

To send parameters with the primary action, call `sendAction()` with the
string `'action'` as the first argument and any additional parameters
following it:

プライマリActionとともにパラメーターを送るには、最初の引数として文字列`"action"`と、次のようななんらかの追加のパラメーターをともなって`sendAction()`を呼び出します。

```js
this.sendAction('action', param1, param2);
```

For example, imagine we're building a todo list that allows the user to
delete a todo:

例えば、ユーザーがTodoを削除できるTodoリストを作ることを想像してください。

```js
App.IndexRoute = Ember.Route.extend({
  model: function() {
    return {
      todos: [{
        title: "Learn Ember.js"
      }, {
        title: "Walk the dog"
      }]
    };
  },

  actions: {
    deleteTodo: function(todo) {
      var todos = this.modelFor('index').todos;
      todos.removeObject(todo);
    }
  }
});
```

```handlebars
{{! index.handlebars }}

{{#each todo in todos}}
  <p>{{todo.title}} <button {{action "deleteTodo" todo}}>Delete</button></p>
{{/each}}
```

We want to update this app so that, before actually deleting a todo, the
user must confirm that this is what they intended. We'll implement a
component that first double-checks with the user before completing the
action.

私たちは、実際にTodoを削除する前に、ユーザーに「本当に削除するつもりなのか」確認するように、このアプリケーションを更新したいとします。Actionを完了する前に、最初にユーザーにダブルチェックをおこなうようにComponentを実装します。

In the component, when triggering the primary action, we'll pass an
additional argument that the component user can specify:

Componentにおいて、プライマリActionをトリガーするときに、Componentのユーザーが指定できる追加の引数を渡します。

```js
App.ConfirmButtonComponent = Ember.Component.extend({
  actions: {
    showConfirmation: function() {
      this.toggleProperty('isShowingConfirmation');
    },

    confirm: function() {
      this.toggleProperty('isShowingConfirmation');
      this.sendAction('action', this.get('param'));
    }
  }
});
```

```handlebars
{{! templates/components/confirm-button.handlebars }}

{{#if isShowingConfirmation}}
  <button {{action "confirm"}}>Click again to confirm</button>
{{else}}
  <button {{action "showConfirmation"}}>{{title}}</button>
{{/if}}
```

Now we can update our initial template and replace the `{{action}}`
helper with our new component:

ここで、私たちは初期のテンプレートを更新し、`{{action}}`ヘルパーを私たちの新しいComponentで置き換えることができます。

```handlebars
{{! index.handlebars }}

    {{#each todo in todos}}
      <p>{{todo.title}} {{confirm-button title="Delete" action="deleteTodo" param=todo}}</p>
    {{/each}}
```

Note that we've specified the action to send by setting the component's
`action` attribute, and we've specified which argument should be sent as
a parameter by setting the component's `param` attribute.

私たちは、Componentの`action`属性をセットすることによってActionを送信するために、このActionを指定したこと、そして、Componentの`param`属性をセットすることによって、どの引数をパラメーターとして送信すべきかを指定したこと、に注目してください。

<a class="jsbin-embed" href="http://jsbin.com/mucilo/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Sending Multiple Actions
### 複数のActionを送る

Depending on the complexity of your component, you may need to let users
specify multiple different actions for different events that your
component can generate.

あなたのComponentの複雑さによっては、あなたのComponentが生成できる異なるイベントのための複数の異なるActionを、ユーザーが指定できるようにする必要があるかもしれません。

For example, imagine that you're writing a form component that the user
can either submit or cancel. Depending on which button the user clicks,
you want to send a different action to your controller or route.

例えば、あなたが、ユーザーが送信またはキャンセルのどちらもできるフォームComponentを書いていると想像してください。あなたは、ユーザーがどちらのボタンをクリックしたかによって、ControllerまたはRouteに異なるActionを送信したいでしょう。

You can specify _which_ action to send by passing the name of the event
as the first argument to `sendAction()`. For example, you can specify two
actions when using the form component:

`sendAction()`の最初の引数として、イベントの名前を渡すことによって、あなたは _どの_ Actionを送信すべきかを指定できます。

```handlebars
{{user-form submit="createUser" cancel="cancelUserCreation"}}
```

In this case, you can send the `createUser` action by calling
`this.sendAction('submit')`, or send the `cancelUserCreation` action by
calling `this.sendAction('cancel')`.

この場合では、`this.sendAction('submit')`を呼び出すことで`createUser`Actionを、または、`this.sendAction('cancel')`を呼び出すことで`cancelUserCreation`Actionを送信できます。

<a class="jsbin-embed" href="http://jsbin.com/qafaq/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Actions That Aren't Specified
### 指定されていないAction

If someone using your component does not specify an action for a
particular event, calling `sendAction()` has no effect.

もし、あなたのComponentを使っている誰かが、特定にイベントについて、Actionを指定しなかった場合は、`sendAction()`の呼び出しは何も効果を起こしません。

For example, if you define a component that triggers the primary action
on click:

例えば、もしあなたが、クリックによってプライマリActionをトリガーするComponentを定義したとすると、

```js
App.MyButtonComponent = Ember.Component.extend({
  click: function() {
    this.sendAction();
  }
});
```

Using this component without assigning a primary action will have no
effect if the user clicks it:

プライマリActionを割り当てないでこのComponentを使うと、ユーザーがクリックしても、何も起こりません。

```handlebars
{{my-button}}
```

### Thinking About Component Actions
### ComponentのActionについて考える

In general, you should think of component actions as translating a
_primitive event_ (like a mouse click or an `<audio>` element's `pause`
event) into actions that have meaning within your application.

一般的に、ComponentのActionは、（マウスクリックや`<audio>`要素の`pause`イベントのような） _プリミティブイベント_ を、あなたのアプリケーション内部で意味付けされたActionへ変換することだと考えるべきです。

This allows your routes and controllers to implement action handlers
with names like `deleteTodo` or `songDidPause` instead of vague names
like `click` or `pause` that may be ambiguous to other developers when
read out of context.

この仕組みによって、あなたは、RouteとControllerのActionハンドラーを、他の開発者が文脈を解釈しようとするにはあまりに多義的になりがちな、`click`や`pause`のような曖昧な名前の代わりに、`deleteTodo`や`songDidPause`のような名前で実装できるのです。

Another way to think of component actions is as the _public API_ of your
component. Thinking about which events in your component can trigger
actions in their application is the primary way other developers will
use your component. In general, keeping these events as generic as
possible will lead to components that are more flexible and reusable.

ComponentのActionの別の考え方は、あなたのComponentの _公開API_ として考えることです。他の開発者のアプリケーションにおいて、あなたのComponentのどのイベントがActionをトリガーできるのかを考えることは、あなたのComponentを使おうとする他の開発者にとっての主な方法です。一般的には、これらのイベントを可能な限りジェネリック（一般的）に保つことは、Componentをより柔軟でより再利用可能なものにします。

(The original document’s commit SHA1: 7fa5e25d9f090004615f4b440aaac713638e38dc)