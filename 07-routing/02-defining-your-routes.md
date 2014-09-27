# DEFINING YOUR ROUTES
# ルートの定義

When your application starts, the router is responsible for displaying
templates, loading data, and otherwise setting up application state.
It does so by matching the current URL to the _routes_ that you've
defined.

あなたのアプリケーションが起動するとき、Routerはテンプレートを表示し、データを読み込み、あるいはアプリケーションの状態をセットアップする役割を負います。Routerは、あなたが定義したRouteに現在のURLを対応させることでこれを行います。

The [map](http://emberjs.com/api/classes/Ember.Router.html#method_map) method
of your Ember application's router can be invoked to define URL mappings. When
calling `map`, you should pass a function that will be invoked with the value
`this` set to an object which you can use to create
[routes](http://emberjs.com/guides/routing/defining-your-routes/) and
[resources](http://emberjs.com/guides/routing/defining-your-routes/#toc_resources).

あなたのEmberアプリケーションのRouterのmap(http://emberjs.com/api/classes/Ember.Router.html#method_map)メソッドはURLマッピングを定義するために呼び出すことができます。`map`を呼び出すときは、あなたは、Route(http://emberjs.com/guides/routing/defining-your-routes/)とResource(http://emberjs.com/guides/routing/defining-your-routes/#toc_resources)を作るために使えるオブジェクトをセットされた`this`を伴って呼び出される関数を、渡す必要があります。

```js
App.Router.map(function() {
  this.route("about", { path: "/about" });
  this.route("favorites", { path: "/favs" });
});
```

Now, when the user visits `/about`, Ember.js will render the `about`
template. Visiting `/favs` will render the `favorites` template.

さて、ユーザーが`/about`に訪れたとき、Ember.jsは`about`テンプレートをレンダリングします。`/favs`に訪れた場合は`favorite`テンプレートをレンダリングします。

<aside>
**Heads up!** You get a few routes for free: the `ApplicationRoute` and
the `IndexRoute` (corresponding to the `/` path).
[See below](#toc_initial-routes) for more details.
</aside>

**注意！** あなたは、何もせずにいくつかのRouteを得ます。すなわち、`ApplicationRoute`と`IndexRoute`（`/`パスに対応しています）。より詳細は、[下の記述](#toc_initial-routes)を参照してください。

Note that you can leave off the path if it is the same as the route
name. In this case, the following is equivalent to the above example:

もしパスがRoute名と同じであれば、パスの指定を省くことができます。今回のケースでは、次のコードは上の例と同等です。

```js
App.Router.map(function() {
  this.route("about");
  this.route("favorites", { path: "/favs" });
});
```

Inside your templates, you can use `{{link-to}}` to navigate between
routes, using the name that you provided to the `route` method (or, in
the case of `/`, the name `index`).

テンプレートの中で、`{{link-to}}`ヘルパーに`route`メソッドで指定した名前を渡すことで（あるいは、`/`のときは`index`という名前を渡すことで）、Route間をナビゲートさせることができます。

```handlebars
{{#link-to 'index'}}<img class="logo">{{/link-to}}

<nav>
  {{#link-to 'about'}}About{{/link-to}}
  {{#link-to 'favorites'}}Favorites{{/link-to}}
</nav>
```

The `{{link-to}}` helper will also add an `active` class to the link that
points to the currently active route.

`{{linkTo}}`ヘルパーはまた、現在のアクティブなRouteを指しているリンクに対して、`active`クラスを追加します。

You can customize the behavior of a route by creating an `Ember.Route`
subclass. For example, to customize what happens when your user visits
`/`, create an `App.IndexRoute`:

`Ember.Route`のサブクラスを作る事で、ルートの動作をカスタマイズすることができます。例えば、ユーザーが `/` に訪れたときに何が起こるかをカスタマイズするには、次のように`App.IndexRoute`を作ります。

```javascript
App.IndexRoute = Ember.Route.extend({
  setupController: function(controller) {
    // Set the IndexController's `title`
    controller.set('title', "My App");
  }
});
```

The `IndexController` is the starting context for the `index` template.
Now that you've set `title`, you can use it in the template:

`IndexController`は`index`テンプレートのためのコンテキストを開始します。`title`プロパティをセットしたので、あなたはテンプレートの中で`title`プロパティを使うことができます。

```handlebars
<!-- get the title from the IndexController -->
<h1>{{title}}</h1>
```

(If you don't explicitly define an `App.IndexController`, Ember.js will
automatically generate one for you.)

（もし`App.IndexController`を明示的に定義しなかった場合、Ember.jsはあなたのために自動的にそれを生成します。）

Ember.js automatically figures out the names of the routes and controllers based on
the name you pass to `this.route`.

Ember.jsは、`this.route`メソッドに渡した名前に基づいて、自動的にRouteとControllerの名前を把握します。

<table>
  <thead>
  <tr>
    <th>URL</th>
    <th>Route Name</th>
    <th>Controller</th>
    <th>Route</th>
    <th>Template</th>
  </tr>
  </thead>
  <tr>
    <td><code>/</code></td>
    <td><code>index</code></td>
    <td><code>IndexController</code></td>
    <td><code>IndexRoute</code></td>
    <td><code>index</code></td>
  </tr>
  <tr>
    <td><code>/about</code></td>
    <td><code>about</code></td>
    <td><code>AboutController</code></td>
    <td><code>AboutRoute</code></td>
    <td><code>about</code></td>
  </tr>
  <tr>
    <td><code>/favs</code></td>
    <td><code>favorites</code></td>
    <td><code>FavoritesController</code></td>
    <td><code>FavoritesRoute</code></td>
    <td><code>favorites</code></td>
  </tr>
</table>

### Resources

You can define groups of routes that work with a resource:

Resourceと連携する、Routeのグループを定義することができます。

```javascript
App.Router.map(function() {
  this.resource('posts', { path: '/posts' }, function() {
    this.route('new');
  });
});
```

As with `this.route`, you can leave off the path if it's the same as the
name of the route, so the following router is equivalent:

`this.route`メソッドと同じように、Routeの名前と同じパスは省略することができます。なので次のRouterは先ほどのものと同等です。

```javascript
App.Router.map(function() {
  this.resource('posts', function() {
    this.route('new');
  });
});
```

This router creates three routes:

このRouterは次の３つのRouteを生成します。

<table>
  <thead>
  <tr>
    <th>URL</th>
    <th>Route Name</th>
    <th>Controller</th>
    <th>Route</th>
    <th>Template</th>
  </tr>
  </thead>
  <tr>
    <td><code>/</code></td>
    <td><code>index</code></td>
    <td><code>IndexController</code></td>
    <td><code>IndexRoute</code></td>
    <td><code>index</code></td>
  </tr>
  <tr>
    <td>N/A</td>
    <td><code>posts</code><sup>1</sup></td>
    <td><code>PostsController</code></td>
    <td><code>PostsRoute</code></td>
    <td><code>posts</code></td>
  </tr>
  <tr>
    <td><code>/posts</code></td>
    <td><code>posts.index</code></code></td>
    <td><code>PostsController</code><br>↳<code>PostsIndexController</code></td>
    <td><code>PostsRoute</code><br>↳<code>PostsIndexRoute</code></td>
    <td><code>posts</code><br>↳<code>posts/index</code></td>
  </tr>
  <tr>
    <td><code>/posts/new</code></td>
    <td><code>posts.new</code></td>
    <td><code>PostsController</code><br>↳<code>PostsNewController</code></td>
    <td><code>PostsRoute</code><br>↳<code>PostsNewRoute</code></td>
    <td><code>posts</code><br>↳<code>posts/new</code></td>
  </tr>
</table>

<small><sup>1</sup> Transitioning to `posts` or creating a link to
`posts` is equivalent to transitioning to `posts.index` or linking to
`posts.index`</small>

<small><sup>1</sup> `posts`に遷移することや`posts`へのリンクを作ることは、`posts.index`に遷移することや`posts.index`へのリンクを作ることと同じです。</small>

NOTE: If you define a resource using `this.resource` and **do not** supply
a function, then the implicit `resource.index` route is **not** created. In
that case, `/resource` will only use the `ResourceRoute`, `ResourceController`,
and `resource` template.

注意：もし`this.resource`メソッドを使ったリソースの定義の際に、関数を **供給しない** 場合、暗黙の`resouce.index` Routeは **生成されません** 。その場合、 `/resource`は`ResourceRoute`、`ResouceController`、`resource`テンプレートのみを使います。

Routes nested under a resource take the name of the resource plus their
name as their route name. If you want to transition to a route (either
via `transitionTo` or `{{#link-to}}`), make sure to use the full route
name (`posts.new`, not `new`).

Resourceの下に入れ子にされたRouteは、Resourceの名前にプラスしてRoute自身の名前を取ります。もし（`transitionTo`または`{{#link-to}}`を通して）Routeに遷移したいなら、フルRoute名（`new`ではなく、`posts.new`）を使うようにしてください。

Visiting `/` renders the `index` template, as you would expect.

`/` に訪れる場合は、あなたの期待通り、`index`テンプレートがレンダリングされます。

Visiting `/posts` is slightly different. It will first render the
`posts` template. Then, it will render the `posts/index` template into the
`posts` template's outlet.

`/posts` の場合は、少し違う動作となります。まず、`posts`テンプレートがレンダリングされます。そして、`posts`テンプレートのOutletに`posts/index`テンプレートがレンダリングされます。

Finally, visiting `/posts/new` will first render the `posts` template,
then render the `posts/new` template into its outlet.

最後に、`/posts/new`に訪れた場合は、`posts`テンプレートが最初にレンダリングされ、それから`posts`テンプレートのOutletに`posts/new`テンプレートがレンダリングされます。

NOTE: You should use `this.resource` for URLs that represent a **noun**,
and `this.route` for URLs that represent **adjectives** or **verbs**
modifying those nouns. For example, in the code sample above, when
specifying URLs for posts (a noun), the route was defined with
`this.resource('posts')`. However, when defining the `new` action
(a verb), the route was defined with `this.route('new')`.

注意：URLが **名詞** を示すように`this.resource`を使うべきです。そして、`this.route`の場合は、URLが、これらの名詞を修飾する **形容詞** または **動詞** を示すように、使うべきです。たとえば、上記のサンプルコードでは、posts（名詞）に対応するURLを指定するとき、Routeは`this.resource('posts')`で定義されています。しかしながら、`new` Action（動詞）を定義するときは、Routeは`this.route('new')`で定義されています。

### Dynamic Segments

One of the responsibilities of a resource's route handler is to convert a URL
into a model.

ResourceのRouteハンドラーの責任の一つは、URLをModelに変換することです。

For example, if we have the resource `this.resource('posts');`, our
route handler might look like this:

例えば、もし`this.resource('posts');`というリソースがあったとして、Routeハンドラーが以下のような感じであったとします。

```js
App.PostsRoute = Ember.Route.extend({
  model: function() {
    return this.store.find('posts');
  }
});
```

The `posts` template will then receive a list of all available posts as
its context.

`posts`テンプレートはそのコンテキストで、利用可能な全てのpostsのリストを受け取ります。

Because `/posts` represents a fixed model, we don't need any
additional information to know what to retrieve.  However, if we want a route
to represent a single post, we would not want to have to hardcode every
possible post into the router.

`/posts`は固定のモデルを指しているので、私たちは何を取得するべきか知るためのどのような追加の情報も必要としません。しかし、もしRouteに単一のpostを指すように望むとしたら、私たちはRouterに、可能性のある全てのpostをハードコードしなければならず、そんなことは誰もしたくないはずです。	

Enter _dynamic segments_.

そこで、_Dynamic Segments_ の出番です。

A dynamic segment is a portion of a URL that starts with a `:` and is
followed by an identifier.

Dynamic Segmentは、`:`から始まり、続いて識別子がくる、URLの一部です。

```js
App.Router.map(function() {
  this.resource('posts');
  this.resource('post', { path: '/post/:post_id' });
});

App.PostRoute = Ember.Route.extend({
  model: function(params) {
    return this.store.find('post', params.post_id);
  }
});
```

Because this pattern is so common, the above `model` hook is the
default behavior.

このパターンはとても一般的なので、上記の`model`フックは（Ember.jsの）標準の動作になっています。

For example, if the dynamic segment is `:post_id`, Ember.js is smart
enough to know that it should use the model `App.Post` (with the ID
provided in the URL). Specifically, unless you override `model`, the route will
return `this.store.find('post', params.post_id)` automatically.

例えば、もしDynamic Segmentが`:post_id`なら、Ember.jsは賢いので、（URLに示されているIDから）Modelである`App.Post`を使うべきと判断します。具体的に言うと、あなたが`model`フックを上書きしなければ、Routeは自動的に`this.store.find('post', params.post_id)`を返します。

Not coincidentally, this is exactly what Ember Data expects. So if you
use the Ember router with Ember Data, your dynamic segments will work
as expected out of the box.

偶然の一致ではなく、これはまさにEmber Dataが期待しているものです。だからもしあなたがEmber DataとともにEmber routerを使うのなら、あなたのDynamic Segmentsは難しい設定なしで期待通りに動きます。

If your model does not use the `id` property in the URL, you should
define a serialize method on your route:

もしあなたのModelがURLで`id`プロパティを使わない場合、Routeの中でserializeメソッドを定義すべきです。

```javascript
App.Router.map(function() {
  this.resource('post', {path: '/posts/:post_slug'});
});

App.PostRoute = Ember.Route.extend({
  model: function(params) {
    // the server returns `{ slug: 'foo-post' }`
    return jQuery.getJSON("/posts/" + params.post_slug);
  },

  serialize: function(model) {
    // this will make the URL `/posts/foo-post`
    return { post_slug: model.get('slug') };
  }
});
```

The default `serialize` method inserts the model's `id` into the route's
dynamic segment (in this case, `:post_id`).

標準の`serialize`メソッドはRouteのDynamic SegmentにModelの`id`を挿入します（この場合、`:post_id`です）。

### Nested Resources
### ネストされたResource

You can nest both routes and resources:

あなたはRouteとResource、どちらもネストすることができます。

```javascript
App.Router.map(function() {
  this.resource('post', { path: '/post/:post_id' }, function() {
    this.route('edit');
    this.resource('comments', function() {
      this.route('new');
    });
  });
});
```

This router creates five routes:

このRouterは以下の５つのRouteを生成します。

<div style="overflow: auto">
  <table>
    <thead>
    <tr>
      <th>URL</th>
      <th>Route Name</th>
      <th>Controller</th>
      <th>Route</th>
      <th>Template</th>
    </tr>
    </thead>
    <tr>
      <td><code>/</code></td>
      <td><code>index</code></td>
      <td><code>App.IndexController</code></td>
      <td><code>App.IndexRoute</code></td>
      <td><code>index</code></td>
    </tr>
    <tr>
      <td>N/A</td>
      <td><code>post</code></td>
      <td><code>App.PostController</code></td>
      <td><code>App.PostRoute</code></td>
      <td><code>post</code></td>
    </tr>
    <tr>
      <td><code>/post/:post_id<sup>2</sup></code></td>
      <td><code>post.index</code></td>
      <td><code>App.PostIndexController</code></td>
      <td><code>App.PostIndexRoute</code></td>
      <td><code>post/index</code></td>
    </tr>
    <tr>
      <td><code>/post/:post_id/edit</code></td>
      <td><code>post.edit</code></td>
      <td><code>App.PostEditController</code></td>
      <td><code>App.PostEditRoute</code></td>
      <td><code>post/edit</code></td>
    </tr>
    <tr>
      <td>N/A</td>
      <td><code>comments</code></td>
      <td><code>App.CommentsController</code></td>
      <td><code>App.CommentsRoute</code></td>
      <td><code>comments</code></td>
    </tr>
    <tr>
      <td><code>/post/:post_id/comments</code></td>
      <td><code>comments.index</code></td>
      <td><code>App.CommentsIndexController</code></td>
      <td><code>App.CommentsIndexRoute</code></td>
      <td><code>comments/index</code></td>
    </tr>
    <tr>
      <td><code>/post/:post_id/comments/new</code></td>
      <td><code>comments.new</code></td>
      <td><code>App.CommentsNewController</code></td>
      <td><code>App.CommentsNewRoute</code></td>
      <td><code>comments/new</code></td>
    </tr>
  </table>
</div>


<small><sup>2</sup> `:post_id` is the post's id.  For a post with id = 1, the route will be:
`/post/1`</small>

<small><sup>2</sup>`:post_id`はpostのidです。id=1のpostの場合、Routeは `/post/1` となります。</small>

The `comments` template will be rendered in the `post` outlet.
All templates under `comments` (`comments/index` and `comments/new`) will be rendered in the `comments` outlet.

`comments`テンプレートは`post`のOutletにレンダリングされます。`comments`の下の全てのテンプレート（`comments/index`と`comments/new`）は`comments`のOutletにレンダリングされます。

The route, controller, and view class names for the comments resource are not prefixed with `Post`. Resources
always reset the namespace, ensuring that the classes can be re-used between multiple parent resources and that
class names don't get longer the deeper nested the resources are.

comments ResourceのためのRoute、Controller、Viewのクラス名に`Post`の接頭語が付けられることはありません。Resourceは常にネームスペースをリセットしますので、クラスは複数の親Resourceの間で再利用でき、クラス名はResourceが深くネストされても長くならないのです。

You are also able to create deeply nested resources in order to preserve the namespace on your routes:

あなたはまた、Routeのネームスペースを保持するために、深くネストしたResourceを作成することもできます。

```javascript
App.Router.map(function() {
  this.resource('foo', function() {
    this.resource('foo.bar', { path: '/bar' }, function() {
      this.route('baz'); // This will be foo.bar.baz
    });
  });
});
```

This router creates the following routes:

このRouterは以下のRouteを生成します。

<div style="overflow: auto">
  <table>
    <thead>
    <tr>
      <th>URL</th>
      <th>Route Name</th>
      <th>Controller</th>
      <th>Route</th>
      <th>Template</th>
    </tr>
    </thead>
    <tr>
      <td><code>/</code></td>
      <td><code>index</code></td>
      <td><code>App.IndexController</code></td>
      <td><code>App.IndexRoute</code></td>
      <td><code>index</code></td>
    </tr>
    <tr>
      <td><code>/foo</code></td>
      <td><code>foo.index</code></td>
      <td><code>App.FooIndexController</code></td>
      <td><code>App.FooIndexRoute</code></td>
      <td><code>foo/index</code></td>
    </tr>
    <tr>
      <td><code>/foo/bar</code></td>
      <td><code>foo.bar.index</code></td>
      <td><code>App.FooBarIndexController</code></td>
      <td><code>App.FooBarIndexRoute</code></td>
      <td><code>foo/bar/index</code></td>
    </tr>
    <tr>
      <td><code>/foo/bar/baz</code></td>
      <td><code>foo.bar.baz</code></td>
      <td><code>App.FooBarBazController</code></td>
      <td><code>App.FooBarBazRoute</code></td>
      <td><code>foo/bar/baz</code></td>
    </tr>
  </table>
</div>


### Initial routes
### 初期Route

A few routes are immediately available within your application:

いくつかのRouteが、あなたのアプリケーションで直ちに利用可能になります。

  - `App.ApplicationRoute` is entered when your app first boots up. It renders
    the `application` template.
    
    `App.ApplicationRoute`はあなたのアプリケーションが起動した時に最初に入るRouteです。このRouteは`application`テンプレートをレンダリングします。

  - `App.IndexRoute` is the default route, and will render the `index` template
    when the user visits `/` (unless `/` has been overridden by your own
    custom route).
    
    `App.IndexRoute`は標準のRouteで、ユーザーが`/`に訪れたときに、`index`テンプレートをレンダリングします（`/`があなたた自身のカスタムRouteで上書きされない限り）。

Remember, these routes are part of every application, so you don't need to
specify them in `App.Router.map`.

覚えておいてください。これらのRouteはすべてのアプリケーションにおける一部分です。なので、あなたはこれらを`App.Router.map`で指定する必要はありません。

### Wildcard / globbing routes
### ワイルドカード / globbing Routes

You can define wildcard routes that will match mutliple routes. This could be used, for example,
if you'd like a catchall route which is useful when the user enters an incorrect URL not managed
by your app.

あなたは、複数のRouteにマッチする、ワイルドカードRouteを定義することができます。たとえば、ユーザーがあなたのアプリケーションで管理していない誤ったURLを入力したときなどに便利な、汎用のRouteが欲しいときなどに使うことができます。

```javascript
App.Router.map(function() {
  this.route('catchall', {path: '/*wildcard'});
});
```

Like all routes with a dynamic segment, you must provide a context when using a `{{link-to}}`
or `transitionTo` to programatically enter this route.

Dynamic Segmentがある全てのRouteと同様に、このRouteにプログラム的に入るために`{{link-to}}`や`transitionTo`を使うときには、コンテキストを必ず提供しなければなりません。

```javascript
App.ApplicationRoute = Ember.Route.extend({
  actions: {
    error: function () {
      this.transitionTo('catchall', "application-error");
    }
  }
});
```

With this code, if an error bubbles up to the Application route, your application will enter
the `catchall` route and display `/application-error` in the URL.

このコードによって、もしエラーがApplication Routeに伝播したら、あなたのアプリケーションは`catchall`Routeに入り、そしてURLの`/application-error`を表示することになります。

(The original document’s commit SHA1: 085eb8f2c986e7ea38e0aa1f4ded6d902c3d02d6)