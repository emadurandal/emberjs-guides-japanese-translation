# REDIRECTING
# リダイレクト

### Transitioning and Redirecting
### 遷移とリダイレクト

Calling `transitionTo` from a route or `transitionToRoute` from a controller
will stop any transition currently in progress and start a new one, functioning
as a redirect. `transitionTo` takes parameters and behaves exactly like the [link-to](http://emberjs.com/guides/templates/links) helper:

Routeからの`transitionTo`の呼び出し、またはControllerからの`transitionToRoute`の呼び出しは、現在進行している遷移を中止し、新しい遷移を始めます。つまり、リダイレクトとして機能します。`transitionTo`はパラメーターをトリ、また[link-to](http://emberjs.com/guides/templates/links) ヘルパーとほぼ同様に振る舞います。

* If you transition into a route without dynamic segments that route's `model` hook
will always run.
  もしDynamic SegmentsなしのRouteに遷移する場合、そのRouteの`model`フックは常に実行されます。

* If the new route has dynamic segments, you need to pass either a _model_ or an _identifier_ for each segment.
Passing a model will skip that segment's `model` hook.  Passing an identifier will run the `model` hook and you'll be able to access the identifier in the params. See [Links](http://emberjs.com/guides/templates/links) for more detail.
  もし新しいRouteがDynamic segmentsを持っていた場合、あなたは各Segmentについて、 _Model_ または _identifier_を渡す必要があります。Modelを渡すことで、そのsegmentの`model`フックがスキップされます。identifer（識別子）を渡すことで、`model`フックが実行され、あなたはparamsからそのidentiferにアクセスすることができます。より詳細は[Links](http://emberjs.com/guides/templates/links)を見てください。

### Before the model is known
### Modelが分かる前

If you want to redirect from one route to another, you can do the transition in 
the `beforeModel` hook of your route handler.
もし、あるRouteから他のRouteへリダイレクトさせたいなら、Routeハンドラーに`beforeModel`フックで遷移をすることができます。

```javascript
App.Router.map(function() {
  this.resource('posts');
});

App.IndexRoute = Ember.Route.extend({
  beforeModel: function() {
    this.transitionTo('posts');
  }
});
```

### After the model is known
### Modelが分かった後

If you need some information about the current model in order to decide about
the redirection, you should either use the `afterModel` or the `redirect` hook. They
receive the resolved model as the first parameter and the transition as the second one,
and thus function as aliases. (In fact, the default implementation of `afterModel` just calls `redirect`.)

もし、リダイレクションを決定するために現在のModelの情報が必要であれば、あなたは`afterModel`フックか`redirect`フックのどちらかをつかうべきです。これらのフックは、第一パラメーターとして解決されたModelを、第２のパラメーターとしてtransitionを受け取ります。なので、エイリアスとしての機能です（実際、`afterModel`の標準の実装は単に`redirect`と呼ばれています）。

 ```javascript
App.Router.map(function() {
  this.resource('posts');
  this.resource('post', { path: '/post/:post_id' });
 });

App.PostsRoute = Ember.Route.extend({
  afterModel: function(posts, transition) {
    if (posts.get('length') === 1) {
      this.transitionTo('post', posts.get('firstObject'));
    }
   }
 });
```

When transitioning to the `PostsRoute` if it turns out that there is only one post,
the current transition will be aborted in favor of redirecting to the `PostRoute`
with the single post object being its model.

`PostsRoute`に遷移するとき、ただ一つのpostしか（Modelに）ないことが分かると、単一のPostオブジェクトを伴う`PostRoute`へのリダイレクトが優先され、現在の遷移が中止されることになります。

### Based on other application state
### 他のアプリケーションの状態に基づいた処理

You can conditionally transition based on some other application state.

他のアプリケーションの状態にもとづいて、条件による遷移をすることができます。

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
  beforeModel: function() {
    var lastFilter = this.controllerFor('application').get('lastFilter');
    this.transitionTo('topCharts.' + (lastFilter || 'songs'));
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

この例では、`/`へ移動すると、ユーザーが居たlast fileter URL（lastFilterの部分が他のRoute名に置き換わる）に即座に遷移します。最初は、`/songs` URLに遷移します。

Your route can also choose to transition only in some cases. If the
`beforeModel` hook does not abort or transition to a new route, the remaining
hooks (`model`, `afterModel`, `setupController`, `renderTemplate`) will execute
as usual.

あなたのRouteは、一部のケースに限って遷移するという選択をすることもできます。もし`beforeModel`フックが中断しない、または新しいルートへの遷移を行わない場合、残りのフック（`model`、`afterModel`, `setupController`、`renderTemplate`）は通常通り実行されます。

(The original document’s commit SHA1: adbba425bf2838d8c07308df83252ed221774321)