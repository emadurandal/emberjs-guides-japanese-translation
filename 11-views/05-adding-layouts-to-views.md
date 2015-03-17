# ADDING LAYOUTS TO VIEWS
# Viewのレイアウトを追加する

Views can have a secondary template that wraps their main template. Like templates,
layouts are Handlebars templates that will be inserted inside the
view's tag.

Viewはメインテンプレートをラップする第２のテンプレートを持つことができます。テンプレートのように、レイアウトはViewのタグの中に挿入されるHandlebarsテンプレートです。

To tell a view which layout template to use, set its `layoutName` property.

Viewに、どのレイアウトテンプレートを使うかを伝えるには、ビューの`layoutName`プロパティをセットします。

To tell the layout template where to insert the main template, use the Handlebars `{{yield}}` helper.
The HTML contents of a view's rendered `template` will be inserted where the `{{yield}}` helper is.

レイアウトテンプレートに、どこにメインテンプレートを挿入するかを伝えるには、Handlebarsの`{{yield}}`ヘルパーを使います。ビューのレンダリングされたtempleateのHTMLコンテンツは{{yield}}ヘルパーのある場所に挿入されます。

First, you define the following layout template:

まず、次のレイアウトテンプレートを定義します。

```html
<script type="text/x-handlebars" data-template-name="my_layout">
  <div class="content-wrapper">
    {{yield}}
  </div>
</script>
```

And then the following main template:

そして、次のようなメインテンプレートを定義します。

```html
<script type="text/x-handlebars" data-template-name="my_content">
  Hello, <b>{{view.name}}</b>!
</script>
```

Finally, you define a view, and instruct it to wrap the template with the defined layout:

最後に、Viewを定義します。そしてViewに、定義されたレイアウトでテンプレートを包むように指示します。

```javascript
AViewWithLayout = Ember.View.extend({
  name: 'Teddy',
  layoutName: 'my_layout',
  templateName: 'my_content'
});
```

This will result in view instances containing the following HTML

これは、次のHTMLを含むViewのインスタンスになります。

```html
<div class="content-wrapper">
  Hello, <b>Teddy</b>!
</div>
```


(The original document’s commit SHA1: d0f174e958ad85efbc3ec687480d2ac74a83ebc7)