# HANDLEBARS BASICS
# Handlebarsの基礎

Ember.js uses the [Handlebars templating library](http://www.handlebarsjs.com)
to power your app's user interface. Handlebars templates are just like
regular HTML, but also give you the ability to embed expressions that
change what is displayed.

Ember.jsはあなたのアプリケーションのユーザーインターフェースを強化するために、Handlebarsテンプレートライブラリを使っています。
Handlebarsテンプレートは通常のHTMLにとても似ており、それだけでなく、表示すべきものを変更するような式を埋め込むための能力をあなたに与えてくれます。

We take Handlebars and extend it with many powerful features. It may
help to think of your Handlebars templates as an HTML-like DSL for
describing the user interface of your app. And, once you've told
Ember.js to render a given template on the screen, you don't need to
write any additional code to make sure it keeps up-to-date.

私たちはHandlebarsを採用し、そしてそれを、たくさんの強力な機能を加えることで拡張しました。Handlebarsテンプレートは、あなたのアプリケーションのユーザーインタフェースを記述するための、HTMLライクなDSLと考えるとよいかもしれません。
そして一度、与えられたテンプレートを画面にレンダリングするようにEmber.jsに命じたら、そのテンプレートのレンダリング結果が最新であることを確実にするための、追加のコードなどは一切必要ないのです。

If you'd prefer an indentation-based alternative to Handlebars syntax, 
try [Emblem.js](http://www.emblemjs.com), but make sure you're comfortable
with Handlebars first!

もしあなたがHandlebarsのシンタックスに代わる、インデントベースの代替物を好むなら、Emblem.jsを試してみてください。しかし、まずはじめに、あなたがHandlebarsで満足するかどうかを確かめてください。

### Defining Templates
### テンプレートの定義

If you're not using build tools, you can define your application's main
template inside your HTML by putting it inside a `<script>` tag, like so:

もしあなたがビルドツールを使っていないなら、HTMLの中であなたのアプリケーションのメインテンプレートを、`<script>`タグの中にそれを置くことで定義できます。

```html
<html>
  <body>
    <script type="text/x-handlebars">
      Hello, <strong>{{firstName}} {{lastName}}</strong>!
    </script>
  </body>
</html>
```

This template will be compiled automatically and become your
[application template](http://emberjs.com/guides/templates/the-application-template/),
which will be displayed on the page when your app loads.

このテンプレートは自動的にコンパイルされ、あなたの[applicationテンプレート](http://emberjs.com/guides/templates/the-application-template/)（[日本語訳](https://github.com/emadurandal/emberjs-guides-japanese-translation/blob/master/04-templates/01-the-application-template.md)）になります。このテンプレートはあなたのアプリケーションが読み込まれたとき、ページに表示されます。

You can also define templates by name that can be used later. For
example, you may want to define a reusable control that is used in many
different places in your user interface. To tell Ember.js to save the
template for later, instead of displaying it immediately, you can add
the `data-template-name` attribute:

あなたは、後に使うことができる名前を付けて、テンプレートを定義することもできます。たとえば、あなたが、ユーザーインターフェースの多くの異なる場所で使用される、再利用可能なコントロールを定義したいとしましょう。Ember.jsに、すぐに表示するのではなく、後のためにそのテンプレートをとっておくように命じるために、あなたは`data-template-name`属性を加えることができます。

```html
<html>
  <head>
    <script type="text/x-handlebars" data-template-name="say-hello">
      <div class="my-cool-control">{{name}}</div>
    </script>
  </head>
</html>
```

If you are using build tools to manage your application's assets, most
will know how to precompile Handlebars templates and make them available
to Ember.js.

もしあなたがアプリケーションのアセットを管理するためにビルドツールを使っているのなら、ほとんどの人は、Handlebarsテンプレートをプリコンパイルし、それらをEmber.jsで利用可能にする方法をご存知でしょう。

### Handlebars Expressions
### Handlebarsの式

Each template has an associated _controller_: this is where the template 
finds the properties that it displays.

それぞれのテンプレートは関連するControllerを持っています。このControllerとは、テンプレートが表示すべきプロパティーを探し出す場所なのです。

You can display a property from your controller by wrapping the property
name in curly braces, like this:

次に示すように、波括弧でプロパティ名を囲むことで、Controllerからプロパティーを表示することができます。

```handlebars
Hello, <strong>{{firstName}} {{lastName}}</strong>!
```

This would look up the `firstName` and `lastName` properties from the
controller, insert them into the HTML described in the template, then
put them into the DOM.

テンプレートはControllerから`firstName`プロパティと`lastName`プロパティを探し出し、テンプレートの中に書かれたHTMLにそれらを挿入します。そして、それらをDOMに入れこみます。

By default, your top-most application template is bound to your `ApplicationController`:

標準では、あなたの最上位のapplicationテンプレートはあなたの`ApplicationController`に結びつけられます。

```javascript
App.ApplicationController = Ember.Controller.extend({
  firstName: "Trek",
  lastName: "Glowacki"
});
```

The above template and controller would combine to display the following
rendered HTML:

上記のテンプレートとControllerは、次に示すレンダリングされたHTMLを表示するために結合されます。

```html
Hello, <strong>Trek Glowacki</strong>!
```

These expressions (and the other Handlebars features you will learn
about next) are _bindings aware_. That means that if the values used
by your templates ever change, your HTML will be updated automatically.

これらの式（と、あなたが次に学ぶであろう他のHandlebarsの機能）はバインディングを意識しています。このことは、もしあなたのテンプレートで使われている値が変われば、あなたのHTMLが自動的に更新されることを意味します。

As your application grows in size, it will have many templates, each
bound to different controllers.

あなたのアプリケーションのサイズが増大するにつれ、アプリケーションは、それぞれ異なるControllerにバインドされる、多くのテンプレートを持つようになるでしょう。

(The original document’s commit SHA1: 656381fe1aaaa8a85686db42c8aa80c248032253)