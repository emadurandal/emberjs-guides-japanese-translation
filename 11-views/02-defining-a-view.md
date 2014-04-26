# DEFINING A VIEW
# ビューの定義

You can use `Ember.View` to render a Handlebars template and insert it into the DOM.

Handlebarsテンプレートをレンダリングして、それをDOMに挿入するために、`Ember.View`を使うことができます。

To tell the view which template to use, set its `templateName` property. For example, if I had a `<script>` tag like this:

どのテンプレートを使うかをViewに伝えるには、Viewの`templateName`プロパティをセットします。例えば、もし次のような<`script>`タグがあったとしたら、

```html
<html>
  <head>
    <script type="text/x-handlebars" data-template-name="say-hello">
      Hello, <b>{{view.name}}</b>
    </script>
  </head>
</html>
```

I would set the `templateName` property to `"say-hello"`.

`templateName`プロパティを`”say-hello”`にセットします。

```javascript
var view = Ember.View.create({
  templateName: 'say-hello',
  name: "Bob"
});
```

Note: For the remainder of the guide, the `templateName` property will be omitted from most examples. You can assume that if we show a code sample that includes an Ember.View and a Handlebars template, the view has been configured to display that template via the `templateName` property.

注意：このガイドのこれ以降では、ほとんどの例で`templateName`プロパティを省略します。もし私たちがEmber.ViewとHandlebarsテンプレートを含んだコードサンプルを提示したら、`templateName`プロパティを通して、Viewがすでにそのテンプレートを表示するように設定されていると想定してください。

You can append views to the document by calling `appendTo`:

`appendTo`を呼び出すことで、Viewをドキュメントに追加することができます。

```javascript
view.appendTo('#container');
```

As a shorthand, you can append a view to the document body by calling `append`:

省略型として、`append`を呼ぶ事で、Viewをドキュメントボディに追加することができます。

```javascript
view.append();
```

To remove a view from the document, call `remove`:

ドキュメントからViewを取り除くためには、`remove`を呼んでください。

```javascript
view.remove();
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)