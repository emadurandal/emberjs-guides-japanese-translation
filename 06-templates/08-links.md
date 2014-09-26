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
{{! photos.handlebars }}

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
* At most one model for each [dynamic segment](http://emberjs.com/guides/routing/defining-your-routes/#toc_dynamic-segments).
  By default, Ember.js will replace each segment with the value of the corresponding object's `id` property.
  If there is no model to pass to the helper, you can provide an explicit identifier value instead.
  The value will be filled into the [dynamic segment](/guides/routing/defining-your-routes/#toc_dynamic-segments)
  of the route, and will make sure that the `model` hook is triggered.
  
  各dynamic segmentについて多くても一つのModel。標準では、Ember.jsは各セグメントを、対応するオブジェクトの`id`プロパティの値で置き換えます。ヘルパーに渡すべきModelが無い場合は、代わりに明示的な識別子の値を渡すことができます。この値はRouteの[dynamic segment](/guides/routing/defining-your-routes/#toc_dynamic-segments)へ埋め込まれます。そしてそのRouteの`model`フックが確実にトリガーされます。

* An optional title which will be bound to the `a` title attribute

  `a`タグのタイトル属性にバインドされる、オプションのタイトル。

```handlebars
{{! photos.handlebars }}

{{#link-to 'photo.edit' 1}}
  First Photo Ever
{{/link-to}}
```

### Example for Multiple Segments
### 複数のセグメントの例

If the route is nested, you can supply a model or an identifier for each dynamic
segment.

もしルートがネストされていたら、それぞれのDynamic Segmentにモデルまたは識別子を供給できます。

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
  {{#link-to 'photo.comment' ５ primaryComment}}
    Main Comment for the Next Photo
  {{/link-to}}
</p>
```

In the above example, the model hook for `PhotoRoute` will run with `params.photo_id = 5`.  The `model` hook for
`CommentRoute` _won't_ run since you supplied a model object for the `comment` segment. The comment's id will
populate the url according to `CommentRoute`'s `serialize` hook.

上記の例では、`PhotoRoute`のmodelフックは`params.photo_id = 5`を伴って実行されます。`CommentRoute`の`model`フックは、`comment`セグメントのためにあなたがModelオブジェクトを供給したために、 _実行されません_ 。commentのidは、`CommentRoute`の`serialize`フックに従ってURLに埋め込まれます。

### Using link-to as an inline helper
### インラインヘルパーとしてのlink-toの使用

In addition to being used as a block expression, the `link-to` helper
can also be used in inline form by specifying the link text as the first
argument to the helper:

ブロック表記としての使い方に加えて、`link-to`ヘルパーは、ヘルパーの最初の引数としてリンクテキストを指定することで、インライン形式で使うこともできます。

```handlebars
A link in {{#link-to 'index'}}Block Expression Form{{/link-to}},
and a link in {{link-to 'Inline Form' 'index'}}.
```

The output of the above would be:

上記の出力は次のようになります。

```html
A link in <a href='/'>Block Expression Form</a>,
and a link in <a href='/'>Inline Form</a>.
```

### Adding additional attributes on a link
### linkに追加の属性を加える

When generating a link you might want to set additional attributes for it. You can do this with additional
arguments to the `link-to` helper:
追加の属性をセットしたいとあなたが望むようなリンクを生成するときは、`link-to`ヘルパーに追加の引数を与えることでそれを実現できます。

```handlebars
<p>
  {{link-to 'Edit this photo' 'photo.edit' photo class="btn btn-primary"}}
</p>
```

Many of the common HTML properties you would want to use like `class`, and `rel` will work. When
adding class names, Ember will also apply the standard `ember-view` and possibly `active` class names.

`class`や`rel`のような、あなたが使いたいと思うであろう多くの一般的なHTMLプロパティは動作します。class名を追加するとき、Emberは標準では`ember-view`というクラス名も、さらに場合によっては`active`というクラス名も適用します。

### Replacing history entries
### 履歴エントリーの置き換え

The default behavior for `link-to` is to add entries to the browser's history
when transitioning between the routes. However, to replace the current entry in
the browser's history you can use the `replace=true` option:

Route間を移動するとき、`link-to`の標準の振る舞いでは、ブラウザーの履歴が追加されます。しかし、ブラウザーの履歴の現在のエントリーを置き換えたいなら、あなたは`replace=true`オプションを使うことができます。

```handlebars
<p>
  {{#link-to 'photo.comment' 5 primaryComment replace=true}}
    Main Comment for the Next Photo
  {{/link-to}}
</p>
```

(The original document’s commit SHA1: 510a7735bb5fac27c137a3323dbeb8bb8a41af18)