# RENDERING A TEMPLATE
# テンプレートのレンダリング

One of the most important jobs of a route handler is rendering the
appropriate template to the screen.

Routeハンドラーの最も重要な仕事の一つは、画面に適切なテンプレートをレンダリングすることです。

By default, a route handler will render the template into the closest
parent with a template.

標準では、Routeハンドラーはテンプレートを持つ直近の親の中に、そのテンプレートをレンダリングします。

```js
App.Router.map(function() {
  this.resource('posts');
});

App.PostsRoute = Ember.Route.extend();
```

If you want to render a template other than the one associated with the
route handler, implement the `renderTemplate` hook:

もしRouteハンドラーに関連していないテンプレートをレンダリングしたい場合は、`renderTemplate`フックを実装します。

```js
App.PostsRoute = Ember.Route.extend({
  renderTemplate: function() {
    this.render('favoritePost');
  }
});
```

If you want to use a different controller than the route handler's
controller, pass the controller's name in the `controller` option:

もし、Routeハンドラーのコントローラーと違うコントローラーを使いたい場合は、`controller`オプションの中にそのControllerの名前を渡してください。

```js
App.PostsRoute = Ember.Route.extend({
  renderTemplate: function() {
    this.render({ controller: 'favoritePost' });
  }
});
```

Ember allows you to name your outlets. For instance, this code allows
you to specify two outlets with distinct names:

Emberでは、Outletに名前を付けることができます。例えば、このコードは二つのOutletを異なった名前で指定しています。

```handlebars
<div class="toolbar">{{outlet toolbar}}</div>
<div class="sidebar">{{outlet sidebar}}</div>
```

So, if you want to render your posts into the `sidebar` outlet, use code
like this:

したがって、もし`sidebar` Outletにあなたのpostsをレンダリングしたいなら、次のようなコードを使います。

```js
App.PostsRoute = Ember.Route.extend({
  renderTemplate: function() {
    this.render({ outlet: 'sidebar' });
  }
});
```

All of the options described above can be used together in whatever
combination you'd like:

上述した全てのオプションは、あなたの好むどのような組み合わせでも、一緒に使うことができます。

```js
App.PostsRoute = Ember.Route.extend({
  renderTemplate: function() {
    var controller = this.controllerFor('favoritePost');

    // Render the `favoritePost` template into
    // the outlet `posts`, and display the `favoritePost`
    // controller.
    this.render('favoritePost', {
      outlet: 'posts',
      controller: controller
    });
  }
});
```

If you want to render two different templates into outlets of two different rendered templates of a route:

もし、Routeの、異なる二つのレンダリング済みテンプレートのOutletに、二つの異なるテンプレートをレンダリングしたいのなら、次のようにします。

```js
App.PostRoute = App.Route.extend({
  renderTemplate: function() {
    this.render('favoritePost', {   // the template to render（レンダリングすべきテンプレート）
      into: 'posts',                // the route to render into（レンダリング先のRoute）
      outlet: 'posts',              // the name of the outlet in the route's template（そのRouteのテンプレートのOutletの名前）
      controller: 'blogPost'        // the controller to use for the template（そのテンプレートで使用するコントローラー）
    });
    this.render('comments', {
      into: 'favoritePost',
      outlet: 'comment',
      controller: 'blogPost'
    });
  }
});
```

(The original document’s commit SHA1: f011f84a20841f47438d61ce46d1d39c654b7917)