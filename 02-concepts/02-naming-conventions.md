# NAMING CONVENTIONS
# 命名規則

Ember.js uses naming conventions to wire up your objects without a
lot  of boilerplate. You will want to  use the conventional names
for your routes, controllers and templates.

Ember.jsは、沢山の決まりきったコーディングをすることなくあなたのオブジェクトに接続するために、命名規則を使います。あなたは、ルート、コントローラ、テンプレートに、規約に従った名前を使用したくなるでしょう。

You can usually guess the names, but this guide outlines, in one place, all of the naming conventions.

あなたはたいてい、（規約に従った）名前を推測することができるでしょう。しかし、このガイドではこの場で、命名規則の全てを概説します。

## The Application
## アプリケーション

When your application boots, Ember will look for these objects:

あなたのアプリケーションが起動した時、Emberは以下のオブジェクトを探します。

* `App.ApplicationRoute`
* `App.ApplicationController`
* the `application` template

Ember.js will render the `application` template as the main template.
If `App.ApplicationController` is provided, Ember.js will set an
instance of `App.ApplicationController` as the controller for the
template. This means that the template will get its properties from
the controller.

Ember.jsは`application`テンプレートをメインテンプレートとしてレンダリングしようとします。もし、`App.ApplicationController`が用意されていたら、Ember.jsは`application`テンプレートのコントローラーとして、`App.ApplicationController`のインスタンスをセットします。これは、テンプレートが、自身のプロパティーをコントローラーから取得することを意味します。

If your app provides an `App.ApplicationRoute`, Ember.js will invoke
[the][1] [router's][2] [hooks][3] first, before rendering the
`application` template.

[1]: /guides/routing/specifying-a-routes-model
[2]: /guides/routing/setting-up-a-controller
[3]: /guides/routing/rendering-a-template

もし、あなたのアプリケーションが`App.ApplicationRoute`を提供するなら、Ember.jsは`application`テンプレートをレンダリングする前に、`App.ApplicationRoute`ルーターのフックを最初に呼び出します。

Here's a simple example that uses a route, controller, and template:

ルート、コントローラー、テンプレートを使うシンプルな例を示します。

```javascript
App.ApplicationRoute = Ember.Route.extend({
  setupController: function(controller) {
    // `controller` is the instance of ApplicationController
    controller.set('title', "Hello world!");
  }
});

App.ApplicationController = Ember.Controller.extend({
  appName: 'My First Example'
});
```

```handlebars
<!-- application template -->
<h1>{{appName}}</h1>

<h2>{{title}}</h2>
```

In Ember.js applications, you will always specify your controllers
as **classes**, and the framework is responsible for instantiating
them and providing them to your templates.

Ember.jsアプリケーションでは、あなたは常にコントローラーを **クラス** として指定します。そしてEmber.jsフレームワークはそれらをインスタンス化し、テンプレートにそれらを供給する役割を担います。

This makes it super-simple to test your controllers, and ensures that
your entire application shares a single instance of each controller.

このことはコントローラーをテストすることを非常に簡単にし、あなたのアプリケーション全体が、それぞれのコントローラの単一のインスタンスを共有することを確実にします。

## Simple Routes
## シンプルなルート

Each of your routes will have a controller and a template with the 
same name as the route.

ルートはそれぞれ、ルートと同じ名前のコントローラとテンプレートを持っています。

Let's start with a simple router:

シンプルなルーターから始めましょう。

```javascript
App.Router.map(function() {
  this.route('favorites');
});
```

If your user navigates to `/favorites`, Ember.js will look for these
objects:

もし、あなたのユーザーが`/favorites`に移動したら、Ember.jsは以下のオブジェクトを探します。

* `App.FavoritesRoute`
* `App.FavoritesController`
* the `favorites` template

Ember.js will render the `favorites` template into the `{{outlet}}`
in the `application` template. It will set an instance of the
`App.FavoritesController` as the controller for the template.

Ember.jsは`application`テンプレートの`{{outlet}}`の中に`favorites`テンプレートをレンダリングします。Ember.jsは`favorite`テンプレートのコントローラーとして`App.FavoritesController`のインスタンスをセットします。

If your app provides an `App.FavoritesRoute`, the framework will
invoke it before rendering the template. Yes, this is a bit
repetitive.

もしあなたのアプリケーションが`App.FavoritesRoute`を提供していたら、Ember.jsフレームワークは`favorite`テンプレートをレンダリングする前に`App.FavoritesRoute`を呼び出します。そうです。少々、これは同じ繰り返しですね。

For a route like `App.FavoritesRoute`, you will probably implement
the `model` hook to specify what model your controller will present
to the template.

`App.FavoritesRoute`のようなルートにおいて、コントローラがどんなモデルをテンプレートに渡すかを指定するために、あなたはおそらくモデルフックを実装するでしょう。

Here's an example:

サンプルです。

```javascript
App.FavoritesRoute = Ember.Route.extend({
  model: function() {
    // the model is an Array of all of the posts
    return this.store.find('post');
  }
});
```

In this example, we didn't provide a `FavoritesController`. Because
the model is an Array, Ember.js will automatically supply an instance
of `Ember.ArrayController`, which will present the backing Array as
its model.

この例では、私たちは`FavoritesController`を提供していません。このモデルは配列なので、Ember.jsは自動的にEmber.ArrayControllerのインスタンスを供給するのです。このインスタンスは返ってくる配列を対応するモデルとして提示します。

You can treat the `ArrayController` as if it was the model itself.
This has two major benefits:

あなたは`ArrayController`をあたかもそれがモデルそのものであるかのように扱うことができます。このことは二つの大きな利点を持ちます。

* You can replace the controller's model at any time without having
  to directly notify the view of the change.
  
  あなたは、直接、Viewに変化を通知する必要なく、いつでもコントローラのモデルを置き換えることができます。
  
* The controller can provide additional computed properties or
  view-specific state that do not belong in the model layer. This
  allows a clean separation of concerns between the view, the
  controller and the model.
  
  コントローラーはさらに、モデルレイヤーに属さない、算出済みプロパティ（computed oroperties）またはビュー固有の状態（view-specific state）を供給することができます。これにより、ビュー、コントローラ、モデルの間にある関連のきれいな分離が可能になります。

The template can iterate over the elements of the controller:

テンプレートは、以下のようにコントローラの要素について繰り返すことができます。

```handlebars
<ul>
{{#each controller}}
  <li>{{title}}</li>
{{/each}}
</ul>
```

## Dynamic Segments
## 動的セグメント

If a route uses a dynamic segment, the route's model will be based
on the value of that segment provided by the user.

もしルートが動的セグメント(dynamic segment)を使うと、ルートのモデルはユーザーから提供されたセグメントの値に基づくようになります。

Consider this router definition:

このルーターの定義について考えましょう。

```javascript
App.Router.map(function() {
  this.resource('post', { path: '/posts/:post_id' });
});
```

In this case, the route's name is `post`, so Ember.js will look for
these objects:

この場合は、ルートの名前は`post`です。このとき、Ember.jsは以下のオブジェクトを探します。

* `App.PostRoute`
* `App.PostController`
* the `post` template

Your route handler's `model` hook converts the dynamic `:post_id`
parameter into a model. The `serialize` hook converts a model object
back into the URL parameters for this route (for example, when
generating a link for a model object).

あなたのルートハンドラーの`model`フックは、動的な`:post_id`パラメーターをモデルに変換します。serializeフックはこのルートのために、モデルオブジェクトをURLパラメーターに戻します（例えば、モデルオブジェクトのためにリンクを生成するときなど）。

```javascript
App.PostRoute = Ember.Route.extend({
  model: function(params) {
    return this.store.find('post', params.post_id);
  },

  serialize: function(post) {
    return { post_id: post.get('id') };
  }
});
```

Because this pattern is so common, it is the default for route
handlers.

このパターンはとても一般的なので、このパターンはルートハンドラーのデフォルト仕様になっています。

* If your dynamic segment ends in `_id`, the default `model`
  hook will convert the first part into a model class on the
  application's namespace (`post` becomes `App.Post`). It will
  then call `find` on that class with the value of the dynamic
  segment.
  
  もしあなたの動的セグメント(dynamic segment)が `_id` で終わっているなら、デフォルトの`model`フックは最初の部分（翻訳者補足：post_id なら postの部分）をアプリケーションの名前空間上のモデルクラスに変換します（postはApp.Postになります）。すると`model`フックは、そのクラス上の`find`メソッドを、動的セグメント（dynamic segment）の値を使って呼び出します。

* The default `serialize` hook will pull the dynamic
  segment with the `id` property of the model object.
  
  デフォルトの`serialize`フックはモデルオブジェクトの`id`プロパティで動的セグメント（dynamic segment）を取り出します。

## Route, Controller and Template Defaults
## ルート、コントローラー、テンプレートのデフォルト

If you don't specify a route handler for the `post` route
(`App.PostRoute`), Ember.js  will still render the `post`
template with the app's instance of `App.PostController`.

もしあなたが`post`ルート（`App.PostRoute`）のためにルートハンドラーを指定しないとしても、Ember.jsはそれでもアプリケーションの`App.PostController`のインスタンスで`post`テンプレートをレンダリングします。

If you don't specify the controller (`App.PostController`),
Ember will automatically make one for you based on the return value
of the route's `model` hook. If the model is an Array, you get an
`ArrayController`. Otherwise, you get an `ObjectController`.

もしあなたがコントローラー（`App.PostController`）を指定しなければ、Emberはルートの`model`フックの戻り値に基づいて、あなたのためにコントローラーを自動的に作成します。もしモデルが配列だったら、あなたは`ArrayController`を得ます。そうでなければ、あなたは`ObjectController`を得ます。

If you don't specify a `post` template, Ember.js won't render
anything!

もしあなたが`post`テンプレートを指定しなければ、Ember.jsは何もレンダリングしません！

## Nesting
## ネスティング

You can nest routes under a `resource`.

リソースの下に、ルートを入れ子にすることができます。

```javascript
App.Router.map(function() {
  this.resource('posts', function() { // the `posts` route
    this.route('favorites');          // the `posts.favorites` route
    this.resource('post');            // the `post` route
  });
});
```

A **resource** is the beginning of a route, controller, or template
name. Even though the `post` resource is nested, its route is named
`App.PostRoute`, its controller is named `App.PostController` and its
template is `post`.

リソースはルート、コントローラーまたはテンプレートの名前の始まりです。たとえpostリソースがネストされていても、そのルートは`App.PostRoute`という名前になり、コントローラーは`App.PostController`という名前になり、そしてそのテンプレートは`post`になります。

When you nest a **route** inside a resource, the route name is added
to the resource name, after a `.`.

あなたがリソースの中にルートを入れ子にした時、ルート名がリソース名の`.`（ドット）の後に付加されます。

Here are the naming conventions for each of the routes defined in
this router:

このルーターで定義されたそれぞれのルートの命名規則を次の表に示します。

<table>
  <thead>
  <tr>
    <th>Route Name</th>
    <th>Controller</th>
    <th>Route</th>
    <th>Template</th>
  </tr>
  </thead>
  <tr>
    <td><code>posts</code></td>
    <td><code>PostsController</code></td>
    <td><code>PostsRoute</code></td>
    <td><code>posts</code></td>
  </tr>
  <tr>
    <td><code>posts.favorites</code></td>
    <td><code>PostsFavoritesController</code></td>
    <td><code>PostsFavoritesRoute</code></td>
    <td><code>posts/favorites</code></td>
  </tr>
  <tr>
    <td><code>post</code></td>
    <td><code>PostController</code></td>
    <td><code>PostRoute</code></td>
    <td><code>post</code></td>
  </tr>
</table>

The rule of thumb is to use resources for nouns, and routes for
adjectives (`favorites`) or verbs (`edit`). This ensures that
nesting does not create ridiculously long names, but avoids
collisions with common adjectives and verbs.

大まかな決まりとしては、リソースに名詞を使い、ルートに形容詞（`favorites`など）又は動詞（`edit`など）を使います。この決まりは、ネスティングがばかばかしいほど長い名前を作らずに、しかしそれでいて共通の形容詞や動詞の衝突を避けることを確実なものにします。

## The Index Route
## インデックスルート

At every level of nesting (including the top level), Ember.js
automatically provides a route for the `/` path named `index`.

ネストしている（トップレベルを含む）すべてレベルにおいて、Ember.jsは自動的に`index`と名付けられた `/`（スラッシュ）パスのためのルートを提供します。

For example, if you write a simple router like this:

例えば、もしあなたがこのようなシンプルなルーターを書いたら……

```javascript
App.Router.map(function() {
  this.route('favorites');
});
```

It is the equivalent of:

それは、次のコードと同等です。

```javascript
App.Router.map(function() {
  this.route('index', { path: '/' });
  this.route('favorites');
});
```

If the user visits `/`, Ember.js will look for these objects:

もしユーザーが `/` に訪れたら、Ember.jsは以下のオブジェクトを探します。

* `App.IndexRoute`
* `App.IndexController`
* the `index` template

The `index` template will be rendered into the `{{outlet}}` in the
`application` template. If the user navigates to `/favorites`,
Ember.js will replace the `index` template with the `favorites`
template.

`index`テンプレートは`application`テンプレートの`{{outlet}}`の中にレンダリングされます。もしユーザーが`/favorites`に移動したら、Ember.jsは`index`テンプレートを`favorites`テンプレートに置き換えます。

A nested router like this:

次のような入れ子のルーターは……。

```javascript
App.Router.map(function() {
  this.resource('posts', function() {
    this.route('favorites');
  });
});
```

Is the equivalent of:

次のコードと同等です。

```javascript
App.Router.map(function() {
  this.route('index', { path: '/' });
  this.resource('posts', function() {
    this.route('index', { path: '/' });
    this.route('favorites');
  });
});
```

If the user navigates to `/posts`, the current route will be
`posts.index`. Ember.js will look for objects named:

もしユーザーが`/posts`に移動したら、カレントルートは`posts.index`になります。するとEmber.jsは次の名前のオブジェクトを探します。

* `App.PostsIndexRoute`
* `App.PostsIndexController`
* The `posts/index` template

First, the `posts` template will be rendered into the `{{outlet}}`
in the `application` template. Then, the `posts/index` template
will be rendered into the `{{outlet}}` in the `posts` template.

最初に、`posts`テンプレートがアプリケーションテンプレートの`{{outlet}}`の中にレンダリングされます。次に、`post/index`テンプレートが`posts`テンプレートの`{{outlet}}`の中にレンダリングされます。

If the user then navigates to `/posts/favorites`, Ember.js will
replace the `{{outlet}}` in the `posts` template with the
`posts/favorites` template.

もし、ユーザーが`/posts/favorites`に移動したら、Ember.jsは`posts`テンプレートの`{{outlet}}`を`posts/favorites`テンプレートで置き換えます。

(The original document’s commit SHA1: 19b66ac684b5caef087c7fcd9070edbaafabf1a7)