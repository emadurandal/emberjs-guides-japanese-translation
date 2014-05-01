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

#### Applying Layouts in Practice
#### 練習としてレイアウトを適用する

Layouts are extremely useful when you have a view with a common wrapper and behavior, but its main template might change.
One possible scenario is a Popup View.

あなたが共通のラッパーや振る舞いを持つViewを持っており、しかしそのメインテンプレートが変更されうるとき、レイアウトは極めて有用です。ひとつの可能性のあるシナリオは、ポップアップビューです。

You can define your popup layout template:

あなたはポップアップレイアウトレンプレートを定義できます。

```html
<script type="text/x-handlebars" data-template-name="popup">
  <div class="popup">
    <button class="popup-dismiss">x</button>
    <div class="popup-content">
    {{yield}}
    </div>
  </div>
</script>
```

Then define your popup view:

次に、ポップアップビューを定義します。

```javascript
App.PopupView = Ember.View.extend({
  layoutName: 'popup'
});
```

Now you can re-use your popup with different templates:

これで、あなたは異なるテンプレートでポップアップを再利用することができます。

```html
{{#view App.PopupView}}
  <form>
    <label for="name">Name:</label>
    <input id="name" type="text" />
  </form>
{{/view}}

{{#view App.PopupView}}
  <p> Thank you for signing up! </p>
{{/view}}
```


(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)