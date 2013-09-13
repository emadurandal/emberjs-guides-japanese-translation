# BINDING ELEMENT ATTRIBUTES
# 要素の属性のバインド

In addition to normal text, you may also want to have your templates
contain HTML elements whose attributes are bound to the controller.

通常のテキストに加え、コントローラーにバインドされた属性を持ったHTML要素を含むテンプレートを作りたいことがあるでしょう。

For example, imagine your controller has a property that contains a URL
to an image:

例えば、コントローラーが画像のURLを含むプロパティを持っていると想像してください。

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

If `isAdministrator` is `false`, Handlebars will produce the following
HTML element:

もし`isAdministrator`が`false`なら、Handlebarsは次のようなHTML要素を生成します。

```html
<input type="checkbox" disabled>
```

If `isAdministrator` is `true`, Handlebars will produce the following:

もし`isAdministrator`が`true`ならHandlebarsは次のように生成します。

```html
<input type="checkbox">
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)