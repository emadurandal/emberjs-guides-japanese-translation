# BINDING ELEMENT ATTRIBUTES
# 要素の属性のバインド

In addition to normal text, you may also want to have your templates
contain HTML elements whose attributes are bound to the controller.

通常のテキストに加え、Controllerにバインドされた属性を持ったHTML要素を含むテンプレートを作りたいことがあるでしょう。

For example, imagine your controller has a property that contains a URL
to an image:

例えば、Controllerが画像のURLを含むプロパティを持っていると想像してください。

```handlebars
<div id="logo">
  <img {{bind-attr src=logoUrl}} alt="Logo">
</div>
```

This generates the following HTML:

これは次のようなHTMLを生成します。

```html
<div id="logo">
  <img src="http://www.example.com/images/logo.png" alt="Logo">
</div>
```

If you use `{{bind-attr}}` with a Boolean value, it will add or remove
the specified attribute. For example, given this template:

もし真偽値で`{{bindAttr}}`を使った場合、指定された属性が追加されるかまたは削除されます。
例えば、このようなテンプレートがあった場合、

```handlebars
<input type="checkbox" {{bind-attr disabled=isAdministrator}}>
```

If `isAdministrator` is `true`, Handlebars will produce the following
HTML element:

もし`isAdministrator`が`true`なら、Handlebarsは次のようなHTML要素を生成します。

```html
<input type="checkbox" disabled>
```

If `isAdministrator` is `false`, Handlebars will produce the following:

もし`isAdministrator`が`false`ならHandlebarsは次のように生成します。

```html
<input type="checkbox">
```

### Adding data attributes
### data属性の追加

By default, view helpers do not accept *data attributes*. For example

標準では、Viewヘルパーは *data属性* を受け入れません。例えば、以下の例は

```handlebars
{{#link-to "photos" data-toggle="dropdown"}}Photos{{/link-to}}

{{input type="text" data-toggle="tooltip" data-placement="bottom" title="Name"}}
```

renders the following HTML:

次のようなHTMLをレンダリングします：

```html
<a id="ember239" class="ember-view" href="#/photos">Photos</a>

<input id="ember257" class="ember-view ember-text-field" type="text" title="Name">
```

There are two ways to enable support for data attributes. One way would be to add an 
attribute binding on the view, e.g. `Ember.LinkView` or `Ember.TextField` for the specific attribute:

data属性のサポートを有効にする二つの方法があります。一つは、Viewにバインドする属性を追加することです。例えば、特定の属性のための`Ember.LinkView`や`Ember.TextField`などです。

```javascript
Ember.LinkView.reopen({
  attributeBindings: ['data-toggle']
});

Ember.TextField.reopen({
  attributeBindings: ['data-toggle', 'data-placement']
});
```

Now the same handlebars code above renders the following HTML:

これで、前述の同じhandlebarsコードが次のようなHTMLを生成するようになります：

```html
<a id="ember240" class="ember-view" href="#/photos" data-toggle="dropdown">Photos</a>

<input id="ember259" class="ember-view ember-text-field" 
       type="text" data-toggle="tooltip" data-placement="bottom" title="Name">
```

You can also automatically bind data attributes on the base view with the
following:

また、次のようにして、ベースViewのdata属性を自動的にバインドすることもできます。

```javascript
Ember.View.reopen({
  init: function() {
    this._super();
    var self = this;

    // bind attributes beginning with 'data-'
    Em.keys(this).forEach(function(key) {
      if (key.substr(0, 5) === 'data-') {
        self.get('attributeBindings').pushObject(key);
      }
    });
  }
});
```

Now you can add as many data-attributes as you want without having to specify them by name.

こうすることで、あなたは名前でdata属性を指定することなく、あなたが望むだけの多くのdata属性を加えることができます。

(The original document’s commit SHA1: 11ffd07d26f29585891207971b8bd9e0ae0037aa)