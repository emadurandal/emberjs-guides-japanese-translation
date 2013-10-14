# REDIRECTING
# リダイレクト

If you want to redirect from one route to another, simply implement the
`redirect` hook in your route handler.

もし、あるRouteから他のRouteへリダイレクトさせたいなら、単純にRouteハンドラーに`redirect`フックを実装します。

```javascript
App.Router.map(function() {
  this.resource('posts');
});

App.IndexRoute = Ember.Route.extend({
  redirect: function() {
    this.transitionTo('posts');
  }
});
```

You can conditionally transition based on some other application state.

他のアプリケーションの状態に基づいた、条件付きの遷移をおこなうこともできます。

```javascript
App.Router.map(function() {
  this.resource('topCharts', function() {
    this.route('choose', { path: '/' });
    this.route('albums');
    this.route('songs');
    this.route('artists');
    this.route('playlists');
  });
});

App.TopChartsChooseRoute = Ember.Route.extend({
  redirect: function() {
    var lastFilter = this.controllerFor('application').get('lastFilter');
    this.transitionTo('topCharts.' + lastFilter || 'songs');
  }
});

// Superclass to be used by all of the filter routes below
App.FilterRoute = Ember.Route.extend({
  activate: function() {
    var controller = this.controllerFor('application');
    controller.set('lastFilter', this.templateName);
  }
});

App.TopChartsSongsRoute = App.FilterRoute.extend();
App.TopChartsAlbumsRoute = App.FilterRoute.extend();
App.TopChartsArtistsRoute = App.FilterRoute.extend();
App.TopChartsPlaylistsRoute = App.FilterRoute.extend();
```

In this example, navigating to the `/` URL immediately transitions into
the last filter URL that the user was at. The first time, it transitions
to the `/songs` URL.

この例では、`/`へ移動すると、ユーザーが居たlast fileter URLに即座に遷移します。

Your route can also choose to transition only in some cases. If the
`redirect` hook does not transition to a new route, the remaining hooks
(`model`, `setupController`, `renderTemplate`) will execute as usual.

あなたのRouteは、一部のケースに限って遷移するという選択をすることもできます。もし`redirect`フックが新しいルートへの遷移を行わない場合、残りのフック（`model`、`setupController`、`renderTemplate`）は通常通り実行されます。

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)