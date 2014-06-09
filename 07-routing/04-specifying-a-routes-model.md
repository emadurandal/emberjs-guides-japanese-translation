# SPECIFYING A ROUTE'S MODEL
# RouteのModelの指定

Templates in your application are backed by models. But how do templates
know which model they should display?

あなたのアプリケーションにあるテンプレートはモデルに下支えされています。しかし、テンプレートはどうやって表示すべきモデルを知るのでしょうか？

For example, if you have a `photos` template, how does it know which
model to render?

たとえば、もし`photos`テンプレートがあるとして、そのテンプレートはレンダリングすべきモデルをどうやって知るのでしょうか？

This is one of the jobs of an `Ember.Route`. You can tell a template
which model it should render by defining a route with the same name as
the template, and implementing its `model` hook.

これは`Ember.Route`の仕事のひとつです。あなたは、テンプレートと同じ名前のRouteを定義し、その`model`フックを実装することで、テンプレートにレンダリングすべきモデルを指示することができます。

For example, to provide some model data to the `photos` template, we
would define an `App.PhotosRoute` object:

たとえば、`photos`テンプレートにいくつかのModelデータを供給するために、`App.PhotoRoute`オブジェクトを定義します。

```js
App.PhotosRoute = Ember.Route.extend({
  model: function() {
    return [{
      title: "Tomster",
      url: "http://emberjs.com/images/about/ember-productivity-sm.png"
    }, {
      title: "Eiffel Tower",
      url: "http://emberjs.com/images/about/ember-structure-sm.png"
    }];
  }
});
```

<a class="jsbin-embed" href="http://jsbin.com/oLUTEd/1/embed?js">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Asynchronously Loading Models
### 非同期読み込みモデル

In the above example, the model data was returned synchronously from the
`model` hook. This means that the data was available immediately and
your application did not need to wait for it to load, in this case
because we immediately returned an array of hardcoded data.

上の例では、`model`フックからModelデータが同期的に返されます。これは、データがすぐに取得でき、アプリケーションがロードのために待つ必要がないことを意味します。このケースでは、ハードコードされたデータの配列をすぐに返しているからです。

Of course, this is not always realistic. Usually, the data will not be
available synchronously, but instead must be loaded asynchronously over
the network. For example, we may want to retrieve the list of photos
from a JSON API available on our server.

もちろん、これは必ずしも現実的ではありません。通常、データは同期的に取得できるものではなく、ネットワークを介して非同期に読まなければなりません。たとえば、私達はサーバーで利用できるJSON APIから、写真のリストを取得したいとします。

In cases where data is available asynchronously, you can just return a
promise from the `model` hook, and Ember will wait until that promise is
resolved before rendering the template.

データを非同期に取得するようなケースの場合、あなたは`model`フックからPromiseを返すことができます。そしてEmberは、テンプレートのレンダリングの前にPromiseが解決されるまで待機します。

If you're unfamiliar with promises, the basic idea is that they are
objects that represent eventual values. For example, if you use jQuery's
`getJSON()` method, it will return a promise for the JSON that is
eventually returned over the network. Ember uses this promise object to
know when it has enough data to continue rendering.

もしあなたがPromiseについて詳しくないのであれば、その基本概念はこのように考えてください。Promiseは結果として生ずる値を表現したオブジェクトであると。例えば、もしあなたがjQueryの`getJSON()`メソッドを使うと、このメソッドは、最終的にネットワークを介して返ってくるJSONデータのためのPromiseを返します。Emberは、いつPromiseオブジェクトがレンダリングを続けるために必要な十分なデータを持つようになるのかを知るために、このPromiseオブジェクトを使います。

For more about promises, see [A Word on
Promises](/guides/routing/asynchronous-routing/#toc_a-word-on-promises)
in the Asynchronous Routing guide.

Promiseについての詳細は、Asynchronous Routing guide（非同期ルーティングガイド）の[A Word on Promises（Promiseにおける用語）](http://emberjs.com/guides/routing/asynchronous-routing/#toc_a-word-on-promises)を参照してください。

Let's look at an example in action. Here's a route that loads the most
recent pull requests sent to Ember.js on GitHub:

実際の例をみてみましょう。ここにGitHubのEmber.jsプロジェクトに送られた最近のプルリクエストをロードするRouteがあります。

```js
App.PullRequestsRoute = Ember.Route.extend({
  model: function() {
    return Ember.$.getJSON('https://api.github.com/repos/emberjs/ember.js/pulls');
  }
});
```

While this example looks like it's synchronous, making it easy to read
and reason about, it's actually completely asynchronous. That's because
jQuery's `getJSON()` method returns a promise. Ember will detect the
fact that you've returned a promise from the `model` hook, and wait
until that promise resolves to render the `pullRequest` template.

この例は一見、同期的にみえますが（読むことが容易いのがその理由ですが）、実際は完全に非同期です。なぜなら、jQueryの`getJSON()`メソッドはPromiseを返すからです。Emberはあなたが`model`フックからPromiseを返したということを検知して、`pullRequest`テンプレートをレンダリングするためにPromiseが解決されるまで待機します。

(For more information on jQuery's XHR functionality, see
[jQuery.ajax](http://api.jquery.com/jQuery.ajax/) in the jQuery
documentation.)

（jQueryのXHR機能のより詳細については、jQueryドキュメンテーションの[jQuery.ajax](http://api.jquery.com/jQuery.ajax/)を参照してください。）

Because Ember supports promises, it can work with any persistence
library that uses them as part of its public API. You can also use many
of the conveniences built in to promises to make your code even nicer.

EmberはPromiseをサポートしているので、公開APIの一部として、Promiseを使用する任意のパーシステンス（永続化）ライブラリと連携させることができます。あなたは、コードをより良いものにするために、Promiseに内蔵されている多くの便利な機能を使うこともできます。

For example, imagine if we wanted to modify the above example so that
the template only displayed the three most recent pull requests. We can
rely on promise chaining to modify the data returned from the JSON
request before it gets passed to the template:

例えば、テンプレートが最も新しい３つのプルリクエストのみを表示するように、上述の例を修正したいものと想像してください。
私達は、テンプレートにPromiseが渡される前に、JSONリクエストから返ってくるデータを修正するためにPromise Chainingを使うことができます。

```js
App.PullRequestsRoute = Ember.Route.extend({
  model: function() {
    var url = 'https://api.github.com/repos/emberjs/ember.js/pulls';
    return Ember.$.getJSON(url).then(function(data) {
      return data.splice(0, 3);
    });
  }
});
```

### Setting Up Controllers with the Model
### Modelを使ってControllerをセットアップする

So what actually happens with the value you return from the `model`
hook?

`model`フックからあなたが返す値を使って、実際何が起きるのでしょうか？

By default, the value returned from your `model` hook will be assigned
to the `model` property of the associated controller. For example, if your
`App.PostsRoute` returns an object from its `model` hook, that object
will be set as the `model` property of the `App.PostsController`.

標準では、あなたの`model`フックから返された値は、関連するControllerの`model`プロパティに割り当てられます。例えば、もし`App.PostsRoute`が`model`フックからなんらかのオブジェクトを返すと、そのオブジェクトは`App.PostsController`の`model`プロパティにセットされます。

(This, under the hood, is how templates know which model to render: they
look at their associated controller's `model` property. For example, the
`photos` template will render whatever the `App.PhotosController`'s
`model` property is set to.)

（内部的には、これはいかにしてテンプレートがレンダリングすべきモデルを知るかという話です。テンプレートは関連するコントローラーの`model`プロパティを探します。例えば、`photos`テンプレートは`App.PhotosControllerの`model`プロパティにセットされたものはなんでもレンダリングしようとします。）

See the [Setting Up a Controller guide][1] to learn how to change this
default behavior. Note that if you override the default behavior and do
not set the `model` property on a controller, your template will not
have any data to render!

この標準の動作を変える方法を学ぶために、[Setting Up a Controller guide（Controllerセットアップガイド）][1]を参照してください。もしあなたがこの標準の動作を上書きして、Controllerの`model`プロパティをセットしないのなら、テンプレートはレンダリングすべきデータを持たないことになることに注意してください。

[1]: http://emberjs.com/guides/routing/setting-up-a-controller

### Dynamic Models

Some routes always display the same model. For example, the `/photos`
route will always display the same list of photos available in the
application. If your user leaves this route and comes back later, the
model does not change.

いくつかのRouteは常に同じModelを表示します。たとえば、`/photos`Routeはアプリケーションで利用可能な同じ写真のリストを常に表示します。もしユーザーがこのRouteを離れ、そして後で戻ってきても、Modelは変わりません。

However, you will often have a route whose model will change depending
on user interaction. For example, imagine a photo viewer app. The
`/photos` route will render the `photos` template with the list of
photos as the model, which never changes. But when the user clicks on a
particular photo, we want to display that model with the `photo`
template. If the user goes back and clicks on a different photo, we want
to display the `photo` template again, this time with a different model.

しかし、ユーザー操作によってモデルが変化するようなRouteを持つこともしばしばあるでしょう。たとえば、フォトビューワーアプリケーションを想像してください。`/photos` Routeは、Modelとして決して変化しない写真のリストを使って`photos`テンプレートをレンダリングしようとします。しかし、ユーザーがある写真をクリックしたとき、`photo`テンプレートでそのModelを表示したいとします。もし、ユーザーがページを戻ったり、違う写真をクリックしたら、その違うModelで`photo`テンプレートを再び表示するとします。

In cases like this, it's important that we include some information in
the URL about not only which template to display, but also which model.

このようなケースでは、URLに、どのテンプレートか、だけでなく、どのModelか、についての情報を含めることが重要です。

In Ember, this is accomplished by defining routes with _dynamic segments_.

Emberでは、 _Dynamic Segment_ を使ってRouteを定義することでこれを実現することができます。

A dynamic segment is a part of the URL that is filled in by the current
model's ID. Dynamic segments always start with a colon (`:`). Our photo
example might have its `photo` route defined like this:

Dynamic Segmentは現在のModelのIDが入ったURLの一部です。Dynamic Segmentは常にコロン（`:`）で始まります。私達の写真の例は、以下のように定義された`photo` Routeを持つことになります。

```js
App.Router.map(function() {
  this.resource('photo', { path: '/photos/:photo_id' });
});
```

In this example, the `photo` route has a dynamic segment `:photo_id`.
When the user goes to the `photo` route to display a particular photo
model (usually via the `{{link-to}}` helper), that model's ID will be
placed into the URL automatically.

この例では、`photo` Routeは`:photo_id` Dynamic Segmentを持ちます。ユーザーが特定の写真のModelを表示するために、（通常は`{{link-to}}`ヘルパーを通して）`photo` Routeに移動した時、ModelのIDがURLの中に自動的に配置されます。

See [Links](http://emberjs.com/guides/templates/links) for more information about linking
to a route with a model using the `{{link-to}}` helper.

`{{link-to}}`ヘルパーを使って、Modelを指定してRouteにリンクすることについての、より詳細な情報はこの[リンク]((http://emberjs.com/guides/templates/links))を参照してください。

For example, if you transitioned to the `photo` route with a model whose
`id` property was `47`, the URL in the user's browser would be updated
to:

例えば、もし`id`プロパティが`47`のモデルを伴って`photo` Routeに遷移したら、ユーザーのブラウザーのURLは以下のように更新されます。

```
/photos/47
```

What happens if the user visits your application directly with a URL
that contains a dynamic segment? For example, they might reload the
page, or send the link to a friend, who clicks on it. At that point,
because we are starting the application up from scratch, the actual
JavaScript model object to display has been lost; all we have is the ID
from the URL.

もしユーザーが、Dynamic Segmentを含むURLで直接あなたのアプリケーションに訪れたら、何が起こるでしょうか？　例えば、彼らはページをリロードしたり、リンクを友達に送って、その友達がリンクをクリックしたりするかもしれません。その時点で、私たちはアプリケーションをゼロから立ち上げているので、表示すべき実際のJavaScript Modelオブジェクトは失われています。私たちが持つ全ては、URLのIDのみです。

Luckily, Ember will extract any dynamic segments from the URL for
you and pass them as a hash to the `model` hook as the first argument:

幸いにして、EmberはURLからDynamic Segmentを取り出し、`model`フックの最初の引数にハッシュとしてDynamic Segmentを渡してくれます。

```js
App.Router.map(function() {
  this.resource('photo', { path: '/photos/:photo_id' });
});

App.PhotoRoute = Ember.Route.extend({
  model: function(params) {
    return Ember.$.getJSON('/photos/'+params.photo_id);
  }
});
```

In the `model` hook for routes with dynamic segments, it's your job to
turn the ID (something like `47` or `post-slug`) into a model that can
be rendered by the route's template. In the above example, we use the
photo's ID (`params.photo_id`) to construct a URL for the JSON
representation of that photo. Once we have the URL, we use jQuery to
return a promise for the JSON model data.

Dynamic Segmentを持つRouteの`model`フックにおいて、（`47`とか`post-slug`のような）IDを、Routeのテンプレートを使ってレンダリングできるModelに変換するのは、あなたの仕事です。上述の例では、私達は写真のID（`params.photo_id`）を、その写真のJSONデータへのURLを組み立てるのに使っています。一度、URLを得れば、私達はjQueryを使って、JSONモデルデータのためのPromiseを返します。

Note: A route with a dynamic segment will only have its `model` hook called
when it is entered via the URL. If the route is entered through a transition
(e.g. when using the [link-to][2] Handlebars helper), then a model context is
already provided and the hook is not executed. Routes without dynamic segments
will always execute the model hook.

[2]: http://emberjs.com/guides/templates/links

注意：Dynamic Segmentを持つRouteは、URLを通してRouteにやってきたときのみ、`model`フックを呼び出します。もしRouteがページ遷移（例えば、[link-to][2] Handlebarsヘルパーを使うときなど）によって訪れられたときは、Modelコンテキストはすでに供給されており、`model`フックは実行されません。Dynamic SegmentのないRouteは常にmodelフックを実行します。


### Ember Data

Many Ember developers use a model library to make finding and saving
records easier than manually managing Ajax calls. In particular, using a
model library allows you to cache records that have been loaded,
significantly improving the performance of your application.

多くのEmber開発者は、手動でAjaxコールを管理するよりも簡単に、レコードを検索したり保存したりできるModelライブラリを使います。具体的には、Modelライブラリを使用することで、読み込まれたレコードをキャッシュすることができ、アプリケーションのパフォーマンスを著しく向上させることができます。

One popular model library built for Ember is Ember Data. To learn more
about using Ember Data to manage your models, see the
[Models](http://emberjs.com/guides/models) guide.

Emberのために作られた人気のあるModelライブラリのひとつが、Ember Dataです。Modelを管理するためにEmber Dataを使うことについてより多くを学ぶために、[Models](http://emberjs.com/guides/models)ガイドを参照してください。

(The original document’s commit SHA1: 1e5bb5df41551257444ed37247f9be787c6f6930)