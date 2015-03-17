# MANAGING DEPENDENCIES BETWEEN CONTROLLERS
# コントローラー間の依存性の管理

Sometimes, especially when nesting resources, we find ourselves needing
to have some kind of connection between two controllers. Let's take this
router as an example:

時には（特にResourceを入れ子にしているとき）、私たちは、２つのコントローラーの間の接続のようなものが必要だと感じることがあります。例として、こんなRouterを取り上げましょう：

```javascript
App.Router.map(function() {
  this.resource("post", { path: "/posts/:post_id" }, function() {
    this.resource("comments", { path: "/comments" });
  });
});
```

If we visit a `/posts/1/comments` URL, our `Post` model will get
loaded into a `PostController`'s model, which means it is not directly
accessible in the `CommentsController`. We might however want to display
some information about it in the `comments` template.

もし私たちが`/post/1/comments`というURLに訪れた場合、`Post`モデルが`PostController`のモデルに読み込まれます。このことは、`CommentsController`において、Postモデルが直接アクセス可能ではないことを意味します。しかし、私たちはPostモデルに関する情報を`comments`テンプレートに表示したいのです。

To be able to do this we define our `CommentsController` to `need` the `PostController`
which has our desired `Post` model.

このことを可能にするには、私たちが望む`Post`モデルを持つ`PostController`を必要とする`CommentsController`を定義します。

```javascript
App.CommentsController = Ember.ArrayController.extend({
  needs: "post"
});
```

This tells Ember that our `CommentsController` should be able to access
its parent `PostController`, which can be done via `controllers.post`
(either in the template or in the controller itself).

このコードは、`CommentsController`がその親の`PostController`にアクセス可能であるべきだ、ということをEmberに命じます。そしてこのアクセスは、（テンプレートの中でも、あるいはController自身の中でも）`controllers.post`を通して行えるようになります。

```handlebars
<h1>Comments for {{controllers.post.title}}</h1>

<ul>
  {{#each comment in comments}}
    <li>{{comment.text}}</li>
  {{/each}}
</ul>
```

We can also create an aliased property to give ourselves a shorter way to access
the `PostController` (since it is an `ObjectController`, we don't need
or want the `Post` instance directly).

`PostController`へのアクセスの近道を得るために、別名のプロパティ（Aliased Property）を作ることもできます。
（PostControllerは`ObjectController`のサブクラスなので、私たちは`Post`インスタンスを直接は必要としません。）

```javascript
App.CommentsController = Ember.ArrayController.extend({
  needs: "post",
  post: Ember.computed.alias("controllers.post")
});
```

If you want to connect multiple controllers together, you can specify an
array of controller names:

もし複数のControllerに一緒に接続したいのなら、Controllerの名前の配列を指定できます。

```javascript
App.AnotherController = Ember.Controller.extend({
  needs: ['post', 'comments']
});
```

For more information about dependecy injection and `needs` in Ember.js,
see the [dependency injection guide](http://emberjs.com/guides/understanding-ember/dependency-injection-and-service-lookup).
For more information about aliases, see the API docs for
[aliased properties](http://emberjs.com/api/#method_computed_alias).

依存性の注入とEmber.jsにおける`needs`について、より詳細な情報を知りたいなら、[dependency injection guide](http://emberjs.com/guides/understanding-ember/dependency-injection-and-service-lookup)を参照してください。
エイリアスについてより詳細な情報を知りたいなら、APIドキュメントの[aliased properties](http://emberjs.com/api/#method_computed_alias)を参照してください。

(The original document’s commit SHA1: ec0c79f78dad7b464efc5cb009ecf045a51f6a44)