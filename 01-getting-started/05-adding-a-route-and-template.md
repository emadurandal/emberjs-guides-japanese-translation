# ADDING THE FIRST ROUTE AND TEMPLATE
# 最初のルートとテンプレートの追加

Next, we will create an Ember.js application, a route ('`/`'), and convert our static mockup into a Handlebars template.

次に、Ember.jsアプリケーションとルート（'`/`'）を作り、そして静的なモックアップをHandlebarsテンプレートに変換します。

Inside your `js` directory, add a file for the application at `js/application.js` and a file for the router at `js/router.js`. You may place these files anywhere you like (even just putting all code into the same file), but this guide will assume you have separated them into their own files and named them as indicated.

`js`ディレクトリの中で、アプリケーションのためのファイル`js/application.js`、そしてルーターのためのファイル`js/router.js`を追加します。あなたはこれらのファイルをどこでも好きな場所に配置してよいですが（全てのコードを同一ファイルに押し込めてもかまいません）、このガイドでは、上述した指定の名前のファイルに分離することを想定します。

Inside `js/application.js` add the following code:

`js/application.js`ファイルに、次のコードを追加します。

```javascript
window.Todos = Ember.Application.create();
```

This will create a new instance of `Ember.Application` and make it available as a variable within your browser's JavaScript environment.

このコードでは、`Ember.Application`の新しいインスタンスを作成し、それをブラウザーのJavaScript環境の中にある変数として利用可能にします。

Inside `js/router.js` add the following code:

`js/router.js`ファイルに次のコードを追加します。

```javascript
Todos.Router.map(function () {
  this.resource('todos', { path: '/' });
});
```

This will tell Ember.js to detect when the application's URL matches `'/'` and to render the `todos` template.

このコードはEmber.jsに、アプリケーションのURLが`'/'`にマッチしたことを検出させ、また`todos`テンプレートをレンダリングさせることを指示します。

Next, update your `index.html` to wrap the inner contents of `<body>` in a Handlebars script tag and include `js/application.js` and `js/router.js`:

次に、`<body>`の中の内容をHandlebarsのスクリプトタグで囲んで、そして`js/application.js`と`js/router.js`をインクルードするように`index.html`を更新します。

```html
<!-- ... additional lines truncated for brevity ... -->
<body>
  <script type="text/x-handlebars" data-template-name="todos">

    <section id="todoapp">
      ... additional lines truncated for brevity ...
    </section>

    <footer id="info">
      <p>Double-click to edit a todo</p>
    </footer>
  
  </script>

  <script src="js/libs/jquery.min.js"></script>
  <script src="js/libs/handlebars.js"></script>
  <script src="js/libs/ember.js"></script>
  <script src="js/libs/ember-data.js"></script>
  
  <script src="js/application.js"></script>
  <script src="js/router.js"></script>
<!-- ... additional lines truncated for brevity ... -->
```

Reload your web browser to ensure that all files have been referenced correctly and no errors occur.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/OKEMIJi/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/8775d1bf4c05eb82adf178be4429e5b868ac145b)
  * [Handlebars Guide](/guides/templates/handlebars-basics)
  * [Ember.Application Guide](/guides/application)
  * [Ember.Application API Documentation](/api/classes/Ember.Application.html)
  
(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)
