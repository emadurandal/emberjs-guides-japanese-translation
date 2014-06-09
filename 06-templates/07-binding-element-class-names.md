# BINDING ELEMENT CLASS NAMES
# 要素のクラス名のバインド

An HTML element's `class` attribute can be bound like any other
attribute:

HTML要素の`class`属性は他の属性と同じようにバインドできます。

```handlebars
<div {{bind-attr class="priority"}}>
  Warning!
</div>
```

If the controller's `priority` property is `"p4"`, this template will emit the following HTML:

もしControllerの`priority`プロパティが`”p4”`なら、このテンプレートは次のようなHTMLを出力します。

```html
<div class="p4">
  Warning!
</div>
```

### Binding to Boolean Values
### 真偽値へのバインド

If the value to which you bind is a Boolean, Ember.js will apply the
dasherized version of the property name as a class:

あなたがバインドする値が真偽値であれば、Ember.jsはクラス名として、プロパティ名を「ダッシュ化」したものを適用します。

```handlebars
<div {{bind-attr class="isUrgent"}}>
  Warning!
</div>
```

If `isUrgent` is true, this emits the following HTML:

もし`isUrgent`がtrueであれば、次のようなHTMLが出力されます。

```html
<div class="is-urgent">
  Warning!
</div>
```

If `isUrgent` is false, no class name is added:

もし`isUrgent`がfalseなら、class名は追加されません。

```html
<div>
  Warning!
</div>
```

If you want to explicitly provide a class name (instead of Ember.js
dasherizing the property name), use the following syntax:

もし（Ember.jsのプロパティ名のダッシュ化の代わりに）明示的にclassの名前を与えたいなら、次のシンタックスを使ってください。

```handlebars
<div {{bind-attr class="isUrgent:urgent"}}>
  Warning!
</div>
```

Instead of the dasherized name, this will produce:

ダッシュ化された名前の代わりに、次のように名前が出力されます。

```html
<div class="urgent">
  Warning!
</div>
```

You can also specify a class name to add when the property is `false`:

プロパティが`false`の時に、付加するclass名を指定することもできます。

```handlebars
<div {{bind-attr class="isEnabled:enabled:disabled"}}>
  Warning!
</div>
```

In this case, if the `isEnabled` property is `true`, the `enabled`
class will be added. If the property is `false`, the class `disabled`
will be added.

このケースでは、`isEnabled`プロパティが`true`なら、`enabled`クラスが追加され、もしプロパティが`false`なら、`disabled`クラスが追加されます。

This syntax can also be used to add a class if a property is `false`
and remove it if the property is `true`, so this:

このシンタックスは、プロパティが`false`の場合にクラスを追加し、プロパティが`true`の場合にクラスを削除するのにも使えます。なので、この次のコードは、

```handlebars
<div {{bind-attr class="isEnabled::disabled"}}>
  Warning!
</div>
```

Will add the class `disabled` when `isEnabled` is `false` and add no
class if `isEnabled` is `true`.

`isEnabled`が`false`のときは`disabled`クラスを追加し、`isEnabled`が`true`ならクラスを追加しません。

### Static Classes
### 静的クラス

If you need an element to have a combination of static and bound
classes, you should include the static class in the list of bound
properties, prefixed by a colon:

もしあなたが静的なクラスとバインドされたクラスの両方を持つ要素を必要としているなら、バインドされたプロパティのリストの中に、コロンを頭に付けた静的クラスを含めてください。

```handlebars
<div {{bind-attr class=":high-priority isUrgent"}}>
  Warning!
</div>
```

This will add the literal `high-priority` class to the element:

このコードは、要素に文字通りの`high-priority`クラスを追加します。

```html
<div class="high-priority is-urgent">
  Warning!
</div>
```

Bound class names and static class names cannot be combined. The
following example **will not work**:

バインドされたクラス名と静的なクラス名は混在できません。次のような例は **動作しません** 。

```handlebars
<div class="high-priority" {{bind-attr class="isUrgent"}}>
  Warning!
</div>
```

### Binding Multiple Classes
### 複数のクラスをバインドする

Unlike other element attributes, you can bind multiple classes:

他の要素の属性と違い、（class属性では）複数のクラスをバインドすることができます。

```handlebars
<div {{bind-attr class="isUrgent priority"}}>
  Warning!
</div>
```

This works how you would expect, applying the rules described above in
order:

この例は、あなたが期待したように動作します。上述したルールが順番に適用されます。

```html
<div class="is-urgent p4">
  Warning!
</div>
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)