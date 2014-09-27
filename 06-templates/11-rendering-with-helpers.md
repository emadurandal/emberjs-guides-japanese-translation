# RENDERING WITH HELPERS
# ヘルパーを使ったレンダリング

Ember.js provides several helpers that allow you to render other views and templates in different ways.

Ember.jsは様々な異なる方法で他のViewやテンプレートをレンダリングすることを可能にするいくつかのヘルパーを提供しています。

### The `{{partial}}` Helper
### `{{partial}}`ヘルパー

`{{partial}}` takes the template to be rendered as an argument, and renders that template in place.

`{{partial}}`は引数としてレンダリングされるテンプレートを取得し、その場所にテンプレートをレンダリングします。

`{{partial}}` does not change context or scope.  It simply drops the given template into place with the current scope.

`{{partial}}`はコンテキストやスコープを変更しません。`{{partial}}`は単純に現在のスコープで、与えられたテンプレートを自身の場所に配置します。

```handlebars
<script type="text/x-handlebars" data-template-name='_author'>
  Written by {{author.firstName}} {{author.lastName}}
</script>

<script type="text/x-handlebars" data-template-name='post'>
  <h1>{{title}}</h1>
  <div>{{body}}</div>
  {{partial "author"}}
</script>
```

```html
<div>
  <h1>Why You Should Use Ember.JS</h1>
  <div>Because it's awesome!</div>
  Written by Yehuda Katz
</div>
```

The partial's `data-template-name` must start with an underscore (e.g. `data-template-name='_author'` or `data-template-name='foo/_bar'`)

パーシャルの`data-template-name`はアンダースコア(_)から始まらなければなりません。（例えば、`data-template-name='_author'`または`data-template-name='foo/_bar'`）

### The `{{view}}` Helper
### `{{view}}`ヘルパー

This helper works like the partial helper, except instead of providing a template to be rendered within the current template, you provide a view class.  The view controls what template is rendered.

このヘルパーはpartialヘルパーと似た働きをします。しかし、現在のテンプレートの中でレンダリングされるべきテンプレートを供給するのではなく、あなたがViewクラスを供給します。Viewがどのテンプレートがレンダリングされるかを制御します。

```javascript
App.AuthorView = Ember.View.extend({
  // We are setting templateName manually here to the default value
  templateName: "author",

  // A fullName property should probably go on App.Author,
  // but we're doing it here for the example
  fullName: (function() {
    return this.get("author").get("firstName") + " " + this.get("author").get("lastName");
  }).property("firstName","lastName")
})
```

```handlebars
<script type="text/x-handlebars" data-template-name='author'>
  Written by {{view.fullName}}
</script>

<script type="text/x-handlebars" data-template-name='post'>
  <h1>{{title}}</h1>
  <div>{{body}}</div>
  {{view "author"}}
</script>
```

```html
<div>
  <h1>Why You Should Use Ember.JS</h1>
  <div>Because it's awesome!</div>
  Written by Yehuda Katz
</div>
```

When using `{{partial "author"}}`:

`{{partial "author"}}`を使うとき、

* No instance of App.AuthorView will be created
  
  App.AuthorViewのインスタンスは生成されません。

* The given template will be rendered
  
  与えられたテンプレートがレンダリングされます。

When using `{{view "author"}}`:

`{{view "author"}}`を使うとき、

* An instance of App.AuthorView will be created
  
  App.AuthorViewのインスタンスが生成されます。
  
* It will be rendered here, using the template associated with that view (the default template being "author")
  
  Viewに関連づけられたテンプレート（デフォルトのテンプレートは"author"になります）を使ってこのヘルパーはレンダリングされます。

For more information, see [Inserting Views in Templates](http://emberjs.com/guides/views/inserting-views-in-templates/)

より詳細な情報は、[Inserting Views in Templates](http://emberjs.com/guides/views/inserting-views-in-templates/)を参照してください。

### The `{{render}}` Helper
### `{{render}}`ヘルパー

`{{render}}` takes two parameters:

`{{render}}`は二つのパラメーターを取ります。

* The first parameter describes the context to be setup
  
  最初のパラメーターは、セットアップされるべきコンテキストです。
  
* The optional second parameter is a model, which will be passed to the controller if provided
  
  オプションの２つ目のパラメーターはモデルです。Modelがもし渡されれば、それはControllerに渡されます。

`{{render}}` does several things:

`{{render}}`はいくつかのことを行います。

* When no model is provided it gets the singleton instance of the corresponding controller
  
  Modelが渡されなかったとき、`{{render}}`は対応するControllerのシングルトンインスタンスを取得します。
  
* When a model is provided it gets a unique instance of the corresponding controller

  Modelが渡されたとき、`{{render}}`は対応するControllerのユニークなインスタンスを取得します。
  
* Renders the named template using this controller
  
  このControllerを使って、指定されたテンプレートをレンダリングします。
  
* Sets the model of the corresponding controller
  
  対応するControllerのModelをセットします。

Modifying the post / author example slightly:

post / authorの例を少し変更してみましょう。

```handlebars
<script type="text/x-handlebars" data-template-name='author'>
  Written by {{firstName}} {{lastName}}.
  Total Posts: {{postCount}}
</script>

<script type="text/x-handlebars" data-template-name='post'>
  <h1>{{title}}</h1>
  <div>{{body}}</div>
  {{render "author" author}}
</script>
```

```javascript
App.AuthorController = Ember.ObjectController.extend({
  postCount: function() {
    return this.get("model.posts.length");
  }.property("model.posts.[]")
})
```

In this example, render will:

この例では、renderは

* Get an instance of App.AuthorView if that class exists, otherwise uses a default generated view
  
  もし存在していれば、App.AuthorViewクラスのインスタンスを取得し、存在していなければ、デフォルト生成されるViewを使います。
  
* Use the corresponding template (in this case the default of "author")
  
  対応するテンプレートを使います（このケースではデフォルトの"author"を）。
  
* Get (or generate) the singleton instance of AuthorController
  
  AuthorControllerのシングルトンインスタンスを取得または生成します。
  
* Set the AuthorController's model to the 2nd argument passed to render, here the author field on the post
  
  AuthorControllerのModelをrenderに渡された２つ目の引数にセットします。ここではpostのauthorフィールドです。

* Render the template in place, with the context created in the previous steps.
  
  前のステップで作成されたコンテキストで、（`{{render}}`の位置に）テンプレートをレンダリングします。

`{{render}}` does not require the presence of a matching route.

`{{render}}`はマッチしたRouteの存在を要求しません。

`{{render}}` is similar to `{{outlet}}`. Both tell Ember.js to devote this portion of the page to something.

`{{render}}`は`{{outlet}}`に似ています。どちらもEmber.jsに、ページのその部分を何かに充てさせようとします。

`{{outlet}}`: The router determines the route and sets up the appropriate controllers/views/models.
`{{render}}`: You specify (directly and indirectly) the appropriate controllers/views/models.

`{{outlet}}`：RouterはControllerを決定し、適切なController／View／Modelをセットアップします。
`{{render}}`：あなたが（直接的または間接的に）適切なController／View／Modelを指定します。



Note: `{{render}}` cannot be called multiple times for the same route when not specifying a model.
注意：Modelを指定していないとき、同じRouteで`{{render}}`を複数回呼ぶことはできません。

### Comparison Table
### 比較表

#### General
#### 一般

<table>
  <thead>
  <tr>
    <th>Helper</th>
    <th>Template</th>
    <th>Model</th>
    <th>View</th>
    <th>Controller</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td><code>{{partial}}</code></td>
    <td>Specified Template</td>
    <td>Current Model</td>
    <td>Current View</td>
    <td>Current Controller</td>
  </tr>
  <tr>
    <td><code>{{view}}</code></td>
    <td>View's Template</td>
    <td>Current Model</td>
    <td>Specified View</td>
    <td>Current Controller</td>
  </tr>
  <tr>
    <td><code>{{render}}</code></td>
    <td>View's Template</td>
    <td>Specified Model</td>
    <td>Specified View</td>
    <td>Specified Controller</td>
  </tr>
  </tbody>
</table>

#### Specific
#### 具体例

<table>
  <thead>
  <tr>
    <th>Helper</th>
    <th>Template</th>
    <th>Model</th>
    <th>View</th>
    <th>Controller</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td><code>{{partial "author"}}</code></td>
    <td><code>author.hbs</code></td>
    <td>Post</td>
    <td><code>App.PostView</code></td>
    <td><code>App.PostController</code></td>
  </tr>
  <tr>
    <td><code>{{view "author"}}</code></td>
    <td><code>author.hbs</code></td>
    <td>Post</td>
    <td><code>App.AuthorView</code></td>
    <td><code>App.PostController</code></td>
  </tr>
  <tr>
    <td><code>{{render "author" author}}</code></td>
    <td><code>author.hbs</code></td>
    <td>Author</td>
    <td><code>App.AuthorView</code></td>
    <td><code>App.AuthorController</code></td>
  </tr>
  </tbody>
</table>

(The original document’s commit SHA1: 535d1eeca3cc6efbe179e5ad0e5460e14305e358)
