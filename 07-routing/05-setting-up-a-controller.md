# SETTING UP A CONTROLLER
# Controllerのセットアップ

Changing the URL may also change which template is displayed on
screen. Templates, however, are usually only useful if they have some
source of information to display.

URLを変更することは、どのテンプレートが画面に表示されるかをも変更することになります。しかし、通常テンプレートが有用なのは、表示すべき情報のソースを持っているときのみです。

In Ember.js, a template retrieves information to display from a
controller.

Ember.jsでは、テンプレートは表示のための情報をControllerから読み出します。

Two built-in controllers—`Ember.ObjectController` and
`Ember.ArrayController`—make it easy for a controller to present a
model's properties to a template, along with any additional
display-specific properties.

二つのビルトインController（`Ember.ObjectController`と`Ember.ArrayController`）は、Controllerがテンプレートに、（追加の表示固有のプロパティと一緒に）Modelのプロパティを渡すことを容易にします。

To tell one of these controllers which model to present, set its
`model` property in the route handler's `setupController` hook.

これらのControllerのうちの一つに、どのModelを渡すかを伝えるために、Routeハンドラーの`setupController`フックの中で、Controllerの`model`プロパティをセットします。

```js
App.Router.map(function() {
  this.resource('post', { path: '/posts/:post_id' });
});

App.PostRoute = Ember.Route.extend({
  // The code below is the default behavior, so if this is all you
  // need, you do not need to provide a setupController implementation
  // at all.
  // 以下のコードは標準の動作です。なのでもしこの動作があなたが望むすべてなら、setupControllerの実装はまったくする必要ありません。
  setupController: function(controller, model) {
    controller.set('model', model);
  }
});
```

The `setupController` hook receives the route handler's associated
controller as its first argument. In this case, the `PostRoute`'s
`setupController` receives the application's instance of
`App.PostController`.

`setupController`フックは、その第一引数として、Routeハンドラーに関連したコントローラーを受け取ります。今回のケースの場合、`PostRoute`の`setupController`はアプリケーションの`App.PostController`のインスタンスを受け取ります。

To specify a controller other than the default, set the route's
`controllerName` property:

標準以外のControllerを指定するには、Routeの`controllerName`プロパティをセットします。

```js
App.SpecialPostRoute = Ember.Route.extend({
  controllerName: 'post'
});
```

As a second argument, it receives the route handler's model. For more
information, see [Specifying a Route's Model][1].

[1]: http://emberjs.com/guides/routing/specifying-a-routes-model

二つ目の引数として、`setupController`フックはRouteハンドラーのModelを受け取ります。より詳細な情報については、[Specifying a Route’s Model（RouteのModelの指定）][1]を参照してください。

The default `setupController` hook sets the `model` property of the
associated controller to the route handler's model.

標準の`setupController`フックは、RouteハンドラーのModelを関連するControllerの`model`プロパティにセットします。

If you want to configure a controller other than the controller
associated with the route handler, use the `controllerFor` method:

もしRouteハンドラーに関連したController以外のControllerを設定したいのであれば、`controllerFor`メソッドを使ってください。

```js
App.PostRoute = Ember.Route.extend({
  setupController: function(controller, model) {
    this.controllerFor('topPost').set('model', model);
  }
});
```

(The original document’s commit SHA1: 8394c83c43ba22d46ae861190a2ae86c49e6384e)