# CUSTOMIZING A COMPONENT'S ELEMENT
# コンポーネントの要素のカスタマイズ

By default, each component is backed by a `<div>` element. If you were
to look at a rendered component in your developer tools, you would see
a DOM representation that looked something like:

標準では、それぞれのコンポーネントは<div>要素で囲まれます。もし開発者ツールでレンダリングされたコンポーネントを見てみると、たとえば次のようなDOM構造が見れるでしょう。

```html
<div id="ember180" class="ember-view">
  <h1>My Component</h1>
</div>
```

You can customize what type of element Ember generates for your
component, including its attributes and class names, by creating a
subclass of `Ember.Component` in your JavaScript.

あなたのJavaScriptの中に`Ember.Component`のサブクラスを作成することで、コンポーネントのためにEmberが生成する要素の種類をカスタマイズすることができます。カスタマイズの対象には、要素の属性やクラス名も含まれます。

### Customizing the Element
### 要素のカスタマイズ

To use a tag other than `div`, subclass `Ember.Component` and assign it
a `tagName` property. This property can be any valid HTML5 tag name as a
string.

`div`以外のタグを使うためには、`Ember.Component`のサブクラスを作り、次に`tagName`プロパティをそのサブクラスに設定します。このプロパティには、文字列として、有効なHTML5タグであればなんでも使うことができます。

```js
App.NavigationBarComponent = Ember.Component.extend({
  tagName: 'nav'
});
```

```handlebars
{{! templates/components/navigation-bar }}
<ul>
  <li>{{#link-to 'home'}}Home{{/link-to}}</li>
  <li>{{#link-to 'about'}}About{{/link-to}}</li>
</ul>
```

### Customizing Class Names
### クラス名のカスタマイズ

You can also specify which class names are applied to the component's
element by setting its `classNames` property to an array of strings:

`classNames`プロパティに文字列の配列をセットすることで、コンポーネントの要素に適用するクラス名を指定することもできます。

```javascript
App.NavigationBarComponent = Ember.Component.extend({
  classNames: ['primary']
});
```

If you want class names to be determined by properties of the component,
you can use class name bindings. If you bind to a Boolean property, the
class name will be added or removed depending on the value:

クラス名をコンポーネントのプロパティによって決定したいのであれば、クラス名バインディングを使うことができます。もし真偽値プロパティにバインドしたら、その値に応じてクラス名が追加されるか、削除されます。

```js
App.TodoItemComponent = Ember.Component.extend({
  classNameBindings: ['isUrgent'],
  isUrgent: true
});
```

This component would render the following:

このコンポーネントは、次のHTMLをレンダリングします。

```html
<div class="ember-view is-urgent"></div>
```

If `isUrgent` is changed to `false`, then the `is-urgent` class name will be removed.

もし`isUrgent`が`false`に変わったら、`is-urgent`クラス名は取り除かれます。

By default, the name of the Boolean property is dasherized. You can customize the class name
applied by delimiting it with a colon:

標準では、真偽値プロパティの名前はダッシュ化されます。クラス名をコロンで区切ることで、クラス名をカスタマイズすることが可能です。

```javascript
App.TodoItemComponent = Ember.Component.extend({
  classNameBindings: ['isUrgent:urgent'],
  isUrgent: true
});
```

This would render this HTML:

これは次のHTMLをレンダリングします。

```html
<div class="ember-view urgent">
```

Besides the custom class name for the value being `true`, you can also specify a class name which is used when the value is `false`:

真偽値プロパティが`true`のときのカスタムクラス名に加えて、真偽値プロパティが`false`の時に使われるクラス名も指定することができます。

```javascript
App.TodoItemComponent = Ember.Component.extend({
  classNameBindings: ['isEnabled:enabled:disabled'],
  isEnabled: false
});
```

This would render this HTML:

これは次のHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

You can also specify a class which should only be added when the property is
`false` by declaring `classNameBindings` like this:

次のように`classNameBindings`を宣言することで、真偽値プロパティが`false`の時に限って加えたいクラス名を指定することもできます。

```javascript
App.TodoItemComponent = Ember.Component.extend({
  classNameBindings: ['isEnabled::disabled'],
  isEnabled: false
});
```

This would render this HTML:

これは次のHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

If the `isEnabled` property is set to `true`, no class name is added:

もし、`isEnabled`プロパティが`true`にセットされたら、クラス名は追加されません。

```html
<div class="ember-view">
```

If the bound value is a string, that value will be added as a class name without
modification:

もしバインドされた値が文字列だったら、その値がクラス名として、改変なしに追加されます。

```javascript
App.TodoItemComponent = Ember.Component.extend({
  classNameBindings: ['priority'],
  priority: 'highestPriority'
});
```

This would render this HTML:

これは次のHTMLをレンダリングします。

```html
<div class="ember-view highestPriority">
```

### Customizing Attributes
### 属性のカスタマイズ

You can bind attributes to the DOM element that represents a component
by using `attributeBindings`:

`attributeBindings`を使う事で、コンポーネントを表すDOM要素に付加される属性をバインドすることができます。

```javascript
App.LinkItemComponent = Ember.Component.extend({
  tagName: 'a',
  attributeBindings: ['href'],
  href: "http://emberjs.com"
});
```

You can also bind these attributes to differently named properties:

異なる名前のプロパティーに、これらの属性をバインドすることもできます。

```javascript
App.LinkItemComponent = Ember.Component.extend({
  tagName: 'a',
  attributeBindings: ['customHref:href'],
  customHref: "http://emberjs.com"
});
```

### Example
### 例

Here is an example todo application that shows completed todos with a
red background:

これは、赤い背景で完了したTodoを表示するTodoアプリケーションの例です。

<a class="jsbin-embed" href="http://jsbin.com/utonef/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

(The original document’s commit SHA1: a4d1dc630ff7698601753ee7fd18f1048b275318)