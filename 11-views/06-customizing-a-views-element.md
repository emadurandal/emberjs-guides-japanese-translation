# CUSTOMIZING A VIEW'S ELEMENT
# Viewの要素のカスタマイズ

A view is represented by a single DOM element on the page. You can change what kind of element is created by
changing the `tagName` property.

Viewはページ上で単一のDOM要素によって表されます。`tagName`プロパティを変更することで、何の種類の要素が生成されるかを変えることができます。

```javascript
App.MyView = Ember.View.extend({
  tagName: 'span'
});
```

You can also specify which class names are applied to the view by setting its `classNames` property to an array of strings:

また、`classNames`プロパティに文字列の配列をセットすることで、Viewに割り当てられるクラス名を指定することもできます。

```javascript
App.MyView = Ember.View.extend({
  classNames: ['my-view']
});
```

If you want class names to be determined by the state of properties on the view, you can use class name bindings. If you bind to
a Boolean property, the class name will be added or removed depending on the value:

もし、クラス名をViewのプロパティの状態によって決定させたいなら、クラス名バインディングを使うことができます。もし真偽値プロパティにバインドしたなら、そのプロパティの値によって、クラス名が追加されたり取り除かれたりします。

```javascript
App.MyView = Ember.View.extend({
  classNameBindings: ['isUrgent'],
  isUrgent: true
});
```

This would render a view like this:

このコードは、次のようなViewをレンダリングします。

```html
<div class="ember-view is-urgent">
```

If `isUrgent` is changed to `false`, then the `is-urgent` class name will be removed.

もし、`isUrgent`が`false`になったら、`is-urgent`クラス名は取り除かれます。

By default, the name of the Boolean property is dasherized. You can customize the class name
applied by delimiting it with a colon:

標準では、真偽値プロパティの名前はダッシュ化されます。コロンでクラス名を区切ることで、適用されるクラス名をカスタマイズすることができます。

```javascript
App.MyView = Ember.View.extend({
  classNameBindings: ['isUrgent:urgent'],
  isUrgent: true
});
```

This would render this HTML:

このコードは次のHTMLをレンダリングします。

```html
<div class="ember-view urgent">
```

Besides the custom class name for the value being `true`, you can also specify a class name which is used when the value is `false`:

真偽値プロパティの値が`true`のときのカスタムクラス名に加えて、値が`false`の時に使われるクラス名を指定することもできます。

```javascript
App.MyView = Ember.View.extend({
  classNameBindings: ['isEnabled:enabled:disabled'],
  isEnabled: false
});
```

This would render this HTML:
このコードは次のHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

You can also specify to only add a class when the property is `false` by declaring `classNameBindings` like this:

また、`classNameBindings`を次のように宣言することで、真偽値プロパティが`false`の時にだけ追加されるクラス名を指定することもできます。


```javascript
App.MyView = Ember.View.extend({
  classNameBindings: ['isEnabled::disabled'],
  isEnabled: false
});
```

This would render this HTML:

このコードは次のHTMLをレンダリングします。

```html
<div class="ember-view disabled">
```

If the `isEnabled` property is set to `true`, no class name is added:

もし`isEnabled`プロパティが`true`にセットされたら、クラス名は追加されません。

```html
<div class="ember-view">
```


If the bound value is a string, that value will be added as a class name without
modification:

もしバインドされた値が文字列だったら、その値が修正されずに、クラス名として追加されます。

```javascript
App.MyView = Ember.View.extend({
  classNameBindings: ['priority'],
  priority: 'highestPriority'
});
```

This would render this HTML:
このコードは次のHTMLをレンダリングします。

```html
<div class="ember-view highestPriority">
```

#### Attribute Bindings on a View
#### Viewの属性をバインドする

You can bind attributes to the DOM element that represents a view by using `attributeBindings`:

`attributeBindings`を使うことで、Viewを表現するDOM要素に属性をバインドすることができます。

```javascript
App.MyView = Ember.View.extend({
  tagName: 'a',
  attributeBindings: ['href'],
  href: "http://emberjs.com"
});
```

You can also bind these attributes to differently named properties:

これらの属性を異なる名前のプロパティにバインドすることもできます。

```javascript
App.MyView = Ember.View.extend({
  tagName: 'a',
  attributeBindings: ['customHref:href'],
  customHref: "http://emberjs.com"
});
```

### Customizing a View's Element from Handlebars
### Handlebarsが提供しているView要素のカスタマイズ

When you append a view, it creates a new HTML element that holds its content.
If your view has any child views, they will also be displayed as child nodes
of the parent's HTML element.

あなたがViewを追加するとき、Viewはその内容を保持する新しいHTML要素を生成します。もしViewが子Viewを持っていたら、それらも親のHTML要素の子ノードとして表示されます。

By default, new instances of `Ember.View` create a `<div>` element. You can
override this by passing a `tagName` parameter:

標準では、`Ember.View`の新しいインスタンスは`<div>`要素を生成します。`tagName`パラメーターを渡すことで、この動作を上書きすることができます。

```handlebars
{{view App.InfoView tagName="span"}}
```

You can also assign an ID attribute to the view's HTML element by passing an `id` parameter:

`id`パラメーターを渡すことで、ViewのHTML要素にID属性を割り当てることもできます。


```handlebars
{{view App.InfoView id="info-view"}}
```

This makes it easy to style using CSS ID selectors:

このことは、CSSのIDセレクターを使って、要素にスタイルをつけることを容易にします。

```css
/** Give the view a red background. **/
#info-view {
  background-color: red;
}
```

You can assign class names similarly:

同じようにして、クラス名も割り当てることができます。

```handlebars
{{view App.InfoView class="info urgent"}}
```

You can bind class names to a property of the view by using `classBinding` instead of `class`. The same behavior as described in `bind-attr` applies:

`class`の代わりに`classBinding`を使う事で、クラス名をViewのプロパティにバインドすることができます。`bind-attr`を使うことで、（属性についても）同じことができます。

```javascript
App.AlertView = Ember.View.extend({
  priority: "p4",
  isUrgent: true
});
```

```handlebars
{{view App.AlertView classBinding="isUrgent priority"}}
```

This yields a view wrapper that will look something like this:

このコードは次のようなViewラッパーを生み出します。

```html
<div id="ember420" class="ember-view is-urgent p4"></div>
```


(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)