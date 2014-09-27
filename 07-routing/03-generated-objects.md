# GENERATED OBJECTS
# 生成されるオブジェクト

As explained in the [routing guide][1], whenever you define a new route,
Ember.js attempts to find corresponding Route, Controller, View, and Template
classes named according to naming conventions. If an implementation of any of
these objects is not found, appropriate objects will be generated in memory for you.

[1]: http://emberjs.com/guides/routing/defining-your-routes/

[routing guide][1]（[日本語版](https://github.com/emadurandal/emberjs-guides-japanese-translation/blob/master/05-routing/02-defining-your-routes.md)）で説明した通り、新しいRouteを定義するときはいつでも、Ember.jsは命名規則によって名付けられた、対応するRoute、Controller、View、テンプレートのクラスを見つけようと試みます。もしこれらのオブジェクトの実装が見つからなければ、あなたのために適切なオブジェクトがメモリ内に生成されます。

#### Generated routes
#### 生成されるRoute

Given you have the following route:

次のようなRouteがあったとして、

```javascript
App.Router.map(function() {
  this.resource('posts');
});
```

When you navigate to `/posts`, Ember.js looks for `App.PostsRoute`.
If it doesn't find it, it will automatically generate an `App.PostsRoute` for you.

あなたが`/posts`に移動すると、Ember.jsは`App.PostsRoute`を探します。もしそれが見つからなければ、Ember.jsは自動的に`App.PostsRoute`をあなたのために生成します。

##### Custom Generated Routes
##### カスタム生成されるRoute

You can have all your generated routes extend a custom route.  If you define `App.Route`,
all generated routes will be instances of that route.

あなたは生成される全てのRouteをカスタムRouteに拡張することができます。もし`App.Route`を定義したら、すべての生成されたRouteはそのRouteのインスタンスとなります。

#### Generated Controllers
#### 生成されるController

If you navigate to route `posts`, Ember.js looks for a controller called `App.PostsController`.
If you did not define it, one will be generated for you.

もしあなたがRoute `posts`に移動したら、Ember.jsは`App.PostsController`と呼ばれるControllerを探します。もしあなたが`App.PostsController`を定義しなければ、あなたのために`App.PostsController`が生成されます。

Ember.js can generate three types of controllers:
`Ember.ObjectController`, `Ember.ArrayController`, and `Ember.Controller`.

Ember.jsは次の３つのタイプのControllerを生成できます。`Ember.ObjectController`、`Ember.ArrayController`、そして`Ember.Controller`です。

The type of controller Ember.js chooses to generate for you depends on your route's
`model` hook:

あなたのために生成しようとEmber.jsが選ぶControllerのタイプは、あなたのRouteの`model`フックに依存します。

- If it returns an object (such as a single record), an [ObjectController][2] will be generated.
  
  もし`model`フックが（単一のレコードのような）単一のオブジェクトを返すなら、[ObjectController][2]が生成されます。
  
- If it returns an array, an [ArrayController][3] will be generated.
  
  もし`model`フックが配列を返す場合は、[ArrayController][3]が生成されます。
  
- If it does not return anything, an instance of `Ember.Controller` will be generated.
  
  もし`model`フックが何も返さない場合は、`Ember.Controller`のインスタンスが生成されます。

[2]: http://emberjs.com/guides/controllers/representing-a-single-model-with-objectcontroller
[3]: http://emberjs.com/guides/controllers/representing-multiple-models-with-arraycontroller


##### Custom Generated Controllers
##### カスタム生成されるController

If you want to customize generated controllers, you can define your own `App.Controller`, `App.ObjectController`
and `App.ArrayController`.  Generated controllers will extend one of these three (depending on the conditions above).

もし生成されるControllerをカスタマイズしたい場合は、あなた独自の`App.Controller`、`App.ObjectController`、`App.ArrayController`を定義することができます。自動生成されるControllerはこれら３つのうちの一つ（どれになるかは上述の条件に依存します）を拡張します。


#### Generated Views and Templates
#### 生成されるViewとテンプレート

A route also expects a view and a template.  If you don't define a view,
a view will be generated for you.

RouteはViewとテンプレートも期待します。もしViewを定義しないなら、Viewがあなたのために生成されます。

A generated template is empty.
If it's a resource template, the template will simply act
as an `outlet` so that nested routes can be seamlessly inserted.  It is equivalent to:

生成されるテンプレートは空です。もしそれがResourceテンプレートなら、単純に`outlet`として振る舞うので、ネストされたRouteをシームレスに挿入することができます。生成されるResourceテンプレートは次のコードと同等です。

```handlebars
{{outlet}}
```

(The original document’s commit SHA1: 13f24865433d6f5a434800cd3fcda9ff344d3e4d)