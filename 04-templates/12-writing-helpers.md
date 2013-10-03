# WRITING HELPERS
# ヘルパーを書く

Sometimes, you may use the same HTML in your application multiple times. In those cases, you can register a custom helper that can be invoked from any Handlebars template.

時には、あなたのアプリケーションで同じHTMLを複数回使うことがあるでしょう。そうしたケースでは、任意のHandlebarsのテンプレートから呼び出せるカスタムヘルパーを登録することができます。

For example, imagine you are frequently wrapping certain values in a `<span>` tag with a custom class. You can register a helper from your JavaScript like this:

例えば、ある値を、カスタムクラスのついた`<span>`タグで囲むことがたびたびあると想像してください。あなたは次のようなJavaScriptによってヘルパーを登録することができます。

```javascript
Ember.Handlebars.helper('highlight', function(value, options) {
  var escaped = Handlebars.Utils.escapeExpression(value);
  return new Handlebars.SafeString('<span class="highlight">' + escaped + '</span>');
});
```

If you return HTML from a helper, and you don't want it to be escaped,
make sure to return a new `SafeString`. Make sure you first escape any
user data!

もしヘルパーからHTMLを返すのなら、そしてあなたがそれをエスケープすべきと思わなくても、確実にnew `SafeString`を戻してください。そして、最初にユーザーデータを確実にエスケープしてください。

Anywhere in your Handlebars templates, you can now invoke this helper:

これで、あなたはHandlebarsテンプレートの中のどこにでも、このヘルパーを呼び出すことができます。

```handlebars
{{highlight name}}
```
#


and it will output the following:

そして、このヘルパーは次のように出力します。

```html
<span class="highlight">Peter</span>
```

If the `name` property on the current context changes, Ember.js will
automatically execute the helper again and update the DOM with the new
value.

現在のコンテキストの`name`プロパティが変化したら、Ember.jsは自動的にヘルパーを再実行して、新しい値でDOMを更新します。

### Dependencies
### 依存性

Imagine you want to render the full name of an `App.Person`. In this
case, you will want to update the output if the person itself changes,
or if the `firstName` or `lastName` properties change.

`App.Person`のフルネームをレンダリングすることを想像してください。この場合、もしpersonそれ自身が変化したり、または`firstName`や`lastName`プロパティが変化したら、出力を更新したいと思うでしょう。

```js
Ember.Handlebars.helper('fullName', function(person) {
  return person.get('firstName') + ' ' + person.get('lastName');
}, 'firstName', 'lastName');
```

You would use the helper like this:

このヘルパーを次のように使います。

```handlebars
{{fullName person}}
```

Now, whenever the context's person changes, or when any of the
_dependent keys_ change, the output will automatically update.

これで、コンテキストのpersonが変化したり、依存しているキーが変化したときはいつでも、出力が自動的に更新されます。

Both the path passed to the `fullName` helper and its dependent keys may
be full _property paths_ (e.g. `person.address.country`).

`fullName`ヘルパーに渡されるパスと、その依存キーの両方とも、完全なプロパティパスになります（例えば、`person.address.country`）。

### Custom View Helpers
### カスタムViewヘルパー

You may also find yourself rendering your view classes in multiple
places using the `{{view}}` helper. In this case, you can save yourself
some typing by registering a custom view helper.

あなたは、複数の場所で`{{view}}`ヘルパーを使って、Viewクラスをレンダリングする自分に気がつくかもしれません。こうした場合、カスタムViewヘルパーを登録することで、タイプ量を減らすことができます。

For example, let’s say you have a view called `App.CalendarView`.
You can register a helper like this:

例えば、`App.CalendarView`というViewがあるとしましょう。あなたは次のようにヘルパーを登録することができます。

```javascript
Ember.Handlebars.helper('calendar', App.CalendarView);
```

Using `App.CalendarView` in a template then becomes as simple as:

テンプレートの中での`App.CalendarView`の使用が、次のように簡単になります。

```handlebars
{{calendar}}
```

Which is functionally equivalent to, and accepts all the same
arguments as:

これは機能的に次と同じです。そして、すべて同じ引数を受け取ります。

```handlebars
{{view App.CalendarView}}
```

(The original document’s commit SHA1: 94704baba6757c519435e865deeeaee006db2485)