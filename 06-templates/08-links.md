# LINKS
# リンク

## The `{{link-to}}` Helper
## `{{link-to}}`ヘルパー

You create a link to a route using the `{{link-to}}` helper.

`{{link-to}}`ヘルパーを使って、Routeへのリンクを作ります。

```js
App.Router.map(function() {
  this.resource("photos", function(){
    this.route("edit", { path: "/:photo_id" });
  });
});
```

```handlebars
<!-- photos.handlebars -->

<ul>
{{#each photo in photos}}
  <li>{{#link-to 'photos.edit' photo}}{{photo.title}}{{/link-to}}</li>
{{/each}}
</ul>
```

If the model for the `photos` template is a list of three photos, the
rendered HTML would look something like this:

もし、`photos`テンプレートのModelが３つのphotoのリストであれば、レンダリングされるHTMLはこんな感じになるでしょう。

```html
<ul>
  <li><a href="/photos/1">Happy Kittens</a></li>
  <li><a href="/photos/2">Puppy Running</a></li>
  <li><a href="/photos/3">Mountain Landscape</a></li>
</ul>
```

When the rendered link matches the current route, and the same
object instance is passed into the helper, then the link is given
`class="active"`.

レンダリングされたリンクが現在のRouteにマッチしたときは、同じオブジェクトのインスタンスがヘルパーに渡されます。すると、そのリンクには`class=”active”`が与えられます。

The `{{link-to}}` helper takes:

`{{link-to}}`ヘルパーは以下を受け取ります。

* The name of a route. In this example, it would be `index`, `photos`, or
  `photos.edit`.
  
  ルート名。この例では、`index`、`photos`または`photos.edit`です。
* At most one model for each [dynamic segment](/guides/routing/defining-your-routes/#toc_dynamic-segments).
  By default, Ember.js will replace each segment with the
  value of the corresponding object's `id` property.
  
  各dynamic segmentについて多くても一つのModel。標準では、Ember.jsは各セグメントを、対応するオブジェクトの`id`プロパティの値で置き換えます。
* An optional title which will be bound to the `a` title attribute

  `a`タグのタイトル属性にバインドされる、オプションのタイトル。


### Example for Multiple Segments
### 複数のセグメントの例

If the route is nested, you can supply a model for each dynamic
segment.

もしルートがネストされていたら、それぞれのDynamic Segmentにモデルを供給できます。

```js
App.Router.map(function() {
  this.resource("photos", function(){
    this.resource("photo", { path: "/:photo_id" }, function(){
      this.route("comments");
      this.route("comment", { path: "/comments/:comment_id" });
    });
  });
});
```

```handlebars
<!-- photoIndex.handlebars -->

<div class="photo">
  {{body}}
</div>

<p>{{#link-to 'photo.comment' primaryComment}}Main Comment{{/link-to}}</p>
```

If you specify only one model, it will represent the innermost dynamic segment `:comment_id`.
The `:photo_id` segment will use the current photo.

もしあなたがただ一つのモデルを指定した場合は、それは最深部のDynamic Segmentである`:comment_id`を指します。`:photo_id`セグメントは現在のphotoを使用します。

Alternatively, you could pass both a photo and a comment to the helper:

代わりに、ヘルパーにphotoとcommentの両方を渡すことができます。

```handlebars
<p>
  {{#link-to 'photo.comment' nextPhoto primaryComment}}
    Main Comment for the Next Photo
  {{/link-to}}
</p>
```

In this case, the models specified will populate both the `:photo_id`
and `:comment_id`.

このケースでは、指定されたモデルは`:photo_id`と`:comment_id`の両方を値で置き換えます。

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)