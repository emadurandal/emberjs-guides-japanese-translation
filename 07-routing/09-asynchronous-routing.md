# ASYNCHRONOUS ROUTING
# 非同期ルーティング

This section covers some more advanced features of the router and its
capability for handling complex async logic within your app.

このセクションでは、Routerのいくつかの高度な機能と、あなたのアプルケーションの中の複雑な非同期ロジックを扱うためのRouterの能力について触れます。

### A Word on Promises...
### Promiseという用語

Ember's approach to handling asynchronous logic in the router makes
heavy use of the concept of Promises. In short, promises are objects that
represent an eventual value. A promise can either _fulfill_
(successfully resolve the value) or _reject_ (fail to resolve the
value). The way to retrieve this eventual value, or handle the cases
when the promise rejects, is via the promise's `then` method, which
accepts two optional callbacks, one for fulfillment and one for
rejection. If the promise fulfills, the fulfillment handler gets called
with the fulfilled value as its sole argument, and if the promise rejects, 
the rejection handler gets called with a reason for the rejection as its
sole argument. For example:

Routerの中で非同期なロジックを扱うためのEmberのアプローチでは、Promiseという概念を非常によく使います。手短に言えば、Promiseとは、結果として生ずる値を表現するオブジェクトのことです。Promiseは_fulfill_（値の解決に成功する）あるいは_reject_（値の解決に失敗する）のどちらの結果にもなりえます。この「結果として生ずる値」を読み出す、またはPromiseがリジェクトされたケースを扱う方法は、Promiseの`then`メソッドを通じて行います。この`then`メソッドは２つのオプションのコールバックを受け入れます。１つは値の解決に成功した場合と、もうひとつは値の解決に失敗した場合のコールバックです。もし、Promiseが値の解決に成功した場合は、フルフィルメント（fulfillment）ハンドラーがたった１つの引数として解決された値を伴って呼び出されます。そしてもしPromiseがリジェクト（値の解決に失敗した）場合は、リジェクション（rejection）ハンドラーがたった１つの引数としてリジェクトされた理由を伴って呼び出されます。
例えば……。

```js
var promise = fetchTheAnswer();

promise.then(fulfill, reject);

function fulfill(answer) {
  console.log("The answer is " + answer);
}

function reject(reason) {
  console.log("Couldn't get the answer! Reason: " + reason);
}
```

Much of the power of promises comes from the fact that they can be
chained together to perform sequential asynchronous operations:

Promiseのパワーの多くは、シーケンシャルな非同期命令を実行するために、Promiseを互いに連結させることができるという事実から来ています。

```js
// Note: jQuery AJAX methods return promises
var usernamesPromise = Ember.$.getJSON('/usernames.json');

usernamesPromise.then(fetchPhotosOfUsers)
                .then(applyInstagramFilters)
                .then(uploadTrendyPhotoAlbum)
                .then(displaySuccessMessage, handleErrors);
```

In the above example, if any of the methods
`fetchPhotosOfUsers`, `applyInstagramFilters`, or
`uploadTrendyPhotoAlbum` returns a promise that rejects, 
`handleErrors` will be called with
the reason for the failure. In this manner, promises approximate an
asynchronous form of try-catch statements that prevent the rightward
flow of nested callback after nested callback and facilitate a saner
approach to managing complex asynchronous logic in your applications.

上述の例では、`fetchPhotosOfUsers`や `applyInstagramFilters`または`uploadTrendyPhotoAlbum`のいずれかのメソッドが、リジェクトするPromiseを返したら、`handleErrors`メソッドが、失敗の理由を（引数として）伴って呼び出されます。このようにして、Promiseはtry-catch命令文の非同期形式を近似します。この非同期形式は、ネストされたコールバックの後のネストされたコールバックの右方向の流れを防ぎ、また、あなたのアプリケーションの複雑な非同期のロジックを管理する健全な方法を手助けします。

This guide doesn't intend to fully delve into all the different ways
promises can be used, but if you'd like a more thorough introduction,
take a look at the readme for [RSVP](https://github.com/tildeio/rsvp.js), 
the promise library that Ember uses. 

このガイドでは、Promiseが使われる全ての方法について深く立ち入ることはしませんが、もしあなたがより詳細な導入を知りたいならば、Emberが使っているPromiseライブラリである[RSVP](https://github.com/tildeio/rsvp.js)のReadMeを参照してください。

### The Router Pauses for Promises
### RouterはPromiseのために待機する

When transitioning between routes, the Ember router collects all of the
models (via the `model` hook) that will be passed to the route's
controllers at the end of the transition. If the `model` hook (or the related
`beforeModel` or `afterModel` hooks) return normal (non-promise) objects or 
arrays, the transition will complete immediately. But if the `model` hook 
(or the related `beforeModel` or `afterModel` hooks) returns a promise (or 
if a promise was provided as an argument to `transitionTo`), the transition 
will pause until that promise fulfills or rejects.

Route間を遷移するとき、EmberのRouterは、遷移の最後に、（`model`フックを通して）RouteのControllerに渡される、すべてのモデルを集めます。もし`model`フック（または、関連する`beforeModel`または`afterModel`フック）が通常の（Promiseでない）オブジェクトまたは配列を返した場合、遷移は即座に完了します。しかし、もし`model`フック（または、関連する`beforeModel`または`afterModel`フック）がPromiseを返す（またはPromiseが`transitionTo`メソッドの引数として供給された）場合は、遷移はPromiseがフルフィル（解決される）かリジェクトされるまで待機します。

<aside>
**Note:** The router considers any object with a `then` method
defined on it to be a promise.

注意：Routerは`then`メソッドが定義されたオブジェクトはPromiseであると見なします。
</aside>

If the promise fulfills, the transition will pick up where it left off and
begin resolving the next (child) route's model, pausing if it too is a
promise, and so on, until all destination route models have been
resolved. The values passed to the `setupController` hook for each route
will be the fulfilled values from the promises.

もしPromiseがフルフィルしたら、遷移は中断したところから再開し、また次の（子の）RouteのModelの解決を始めます。もしそれもPromiseだった場合は待機します。このようなことが、すべての目的のRouteのModelが解決されるまで続きます。各Routeについて`setupController`フックに渡された値はPromiseからのフルフィルされた値になります。

A basic example:

基本的な例を見てみましょう。

```js
App.TardyRoute = Ember.Route.extend({
  model: function() {
    return new Ember.RSVP.Promise(function(resolve) {
      Ember.run.later(function() {
        resolve({ msg: "Hold Your Horses" });
      }, 3000);
    });
  }, 

  setupController: function(controller, model) {
    console.log(model.msg); // "Hold Your Horses"
  }
});
```

When transitioning into `TardyRoute`, the `model` hook will be called and
return a promise that won't resolve until 3 seconds later, during which time
the router will be paused in mid-transition. When the promise eventually
fulfills, the router will continue transitioning and eventually call
`TardyRoute`'s `setupController` hook with the resolved object.

`TardyRoute`に遷移するとき、`model`フックが呼び出され、そして`model`フックは３秒経つまで値の解決をしないPromiseを返します。その間、Routerは遷移の途中で待機します。Promiseが最終的にフルフィルしたとき、Routerは遷移を継続し、そして最終的に`TardyRoute`の`setupController`フックを解決されたオブジェクトを伴って呼び出します。

This pause-on-promise behavior is extremely valuable for when you need
to guarantee that a route's data has fully loaded before displaying a
new template. 

この「Promiseを待つ」という動作は、Routeのデータが新しいテンプレートが表示される前に完全にロードされたことをあなたが保証する必要があるときに非常に有用です。

### When Promises Reject...
### Promeseがリジェクトされた時

We've covered the case when a model promise fulfills, but what if it rejects? 

これまで、ModelのPromiseがフルフィルした場合のケースを見てきました。では、Promiseがリジェクトされた場合はどうでしょう？

By default, if a model promise rejects during a transition, the transition is
aborted, no new destination route templates are rendered, and an error
is logged to the console.

標準では、もし遷移の間にModelのPromiseがリジェクトされた場合、その遷移は中止されます。新しい目標のRouteのテンプレートはレンダリングされず、コンソールにエラーログが出力されます。

You can configure this error-handling logic via the `error` handler on
the route's `actions` hash. When a promise rejects, an `error` event
will be fired on that route and bubble up to `ApplicationRoute`'s
default error handler unless it is handled by a custom error handler
along the way, e.g.:

Routeの`actions`ハッシュの`error`ハンドラーを通して、このエラーハンドリングロジックを設定することができます。Promiseがリジェクトされたとき、`error`イベントがRoute上で発火されます。それが例えば以下のようなカスタムのエラーハンドラーに途中で処理されないのであれば、イベントは`ApplicationRoute`のデフォルトエラーハンドラーにまで伝播します。

```js
App.GoodForNothingRoute = Ember.Route.extend({
  model: function() {
    return Ember.RSVP.reject("FAIL");
  },

  actions: {
    error: function(reason) {
      alert(reason); // "FAIL"

      // Can transition to another route here, e.g.
      // this.transitionTo('index');

      // Uncomment the line below to bubble this error event:
      // return true;
    }
  }
});
```

In the above example, the error event would stop right at
`GoodForNothingRoute`'s error handler and not continue to bubble. To
make the event continue bubbling up to `ApplicationRoute`, you can
return true from the error handler.

上の例では、エラーイベントは`GoodForNothingRoute`のエラーハンドラーでストップし、それ以上伝播しません。イベントに`ApplicationRoute`まで伝播しつづけさせるには、エラーハンドラーからtrueを返します。

### Recovering from Rejection
### リジェクトからの回復

Rejected model promises halt transitions, but because promises are chainable,
you can catch promise rejects within the `model` hook itself and convert 
them into fulfills that won't halt the transition.

リジェクトされたModelのPromiseはページ遷移を中断します。しかし、Promiseは数珠つなぎにできるので、`model`フック自身の中でPromiseのリジェクトを把握することができ、それらのPromiseを、遷移を中断しないフルフィルに変換することができます。

```js
App.FunkyRoute = Ember.Route.extend({
  model: function() {
    return iHopeThisWorks().then(null, function() {
      // Promise rejected, fulfill with some default value to
      // use as the route's model and continue on with the transition
      return { msg: "Recovered from rejected promise" };
    });
  }
});
```

### beforeModel and afterModel
### beforeModel と afterModel

The `model` hook covers many use cases for pause-on-promise transitions,
but sometimes you'll need the help of the related hooks `beforeModel`
and `afterModel`. The most common reason for this is that if you're
transitioning into a route with a dynamic URL segment via `{{link-to}}` or
`transitionTo` (as opposed to a transition caused by a URL change), 
the model for the route you're transitioning into will have already been
specified (e.g. `{{#link-to 'article' article}}` or
`this.transitionTo('article', article)`), in which case the `model` hook
won't get called. In these cases, you'll need to make use of either
the `beforeModel` or `afterModel` hook to house any logic while the
router is still gathering all of the route's models to perform a
transition.

`model`フックはPromise上で一時停止するページ遷移のための多くの利用ケースに対応します。しかし、ときには関連したフックである`beforeModel` と `afterModel`の助けを必要とするかもしれません。この共通の理由の大半は、もしあなたが（URLの変更によって起こる遷移とは違って）`{{link-to}}`または`transitionTo`を通じてDynamic URL SegmentでRouteへの遷移をしようとしていた場合、あなたが遷移しようとしているRouteのためのModelがすでに特定されている（例えば、`{{#link-to 'article' article}}` または`this.transitionTo('article', article)`などのように）だろうからです。このようなケースでは`model`フックは呼び出されません。これらのケースでは、あなたは、Routerが遷移を実行するためにまだRouteのすべてのModelを集めている間、任意のロジックを内蔵するために`beforeModel` または `afterModel`を利用する必要があります。

#### `beforeModel`

Easily the more useful of the two, the `beforeModel` hook is called
before the router attempts to resolve the model for the given route. In
other words, it is called before the `model` hook gets called, or, if
`model` doesn't get called, it is called before the router attempts to
resolve any model promises passed in for that route.

この２つのうち、より簡単で便利な`beforeModel`フックは、Routerが与えられたRouteのためにModelを解決しようと試みる前に呼ばれます。言い換えれば、このフックは`model`フックが呼ばれる前に呼ばれるか、あるいは`model`フックが呼ばれなかった場合に、RouterがそのRouteに渡された任意のModelのPromiseを解決しようとする前に呼ばれます。

Like `model`, returning a promise from `beforeModel` will pause the
transition until it resolves, or will fire an `error` if it rejects.

`model`フックのように、`beforeModel`からPromiseを返すことで、それが解決されるまでページ遷移が中断されます。あるいは、もしリジェクトされた場合は`error`が発火されます。

The following is a far-from-exhaustive list of use cases in which
`beforeModel` is very handy:

以下のリストは、`beforeModel`がとても便利なことを示すユースケースの完全とはいえないリストです。

- Deciding whether to redirect to another route before performing a
  potentially wasteful server query in `model`
  
  `model`フックにおいて、潜在的に無駄なサーバークエリを実行する前に他のRouteへリダイレクトするかどうかという判断をする

- Ensuring that the user has an authentication token before proceeding
  onward to `model`
  
  `model`フックに進む前にユーザーが認証トークンを持っていることを保証する
  
- Loading application code required by this route 
  
  このRouteによって要求されたアプリケーションのコードをロードする

```js
App.SecretArticlesRoute  = Ember.Route.extend({
  beforeModel: function() {
    if (!this.controllerFor('auth').get('isLoggedIn')) {
      this.transitionTo('login');
    }
  }
});
```

[See the API Docs for `beforeModel`](http://emberjs.com/api/classes/Ember.Route.html#method_beforeModel)

[`beforeModel`のAPIドキュメントを参照してください。](http://emberjs.com/api/classes/Ember.Route.html#method_beforeModel)

#### `afterModel`

The `afterModel` hook is called after a route's model (which might be a
promise) is resolved, and follows the same pause-on-promise semantics as
`model` and `beforeModel`. It is passed the already-resolved model 
and can therefore perform any additional logic that
depends on the fully resolved value of a model.

`afterModel`フックはRouteの（Promiseかもしれない）Modelが解決された後に呼ばれます。そして、`model`と`beforeModel`と同様に「Promise上で一時停止する動作」を行います。このフックにはすでに解決されたModelが渡され、そしてそれゆえにModelの完全に解決された値に依存した、任意の追加のロジックを実行することができます。

```js
App.ArticlesRoute = Ember.Route.extend({
  model: function() {
    // App.Article.find() returns a promise-like object
    // (it has a `then` method that can be used like a promise)
    return App.Article.find();
  },
  afterModel: function(articles) {
    if (articles.get('length') === 1) {
      this.transitionTo('article.show', articles.get('firstObject'));
    }
  }
});
```

You might be wondering why we can't just put the `afterModel` logic
into the fulfill handler of the promise returned from `model`; the
reason, as mentioned above, is that transitions initiated 
via `{{link-to}}` or `transitionTo` likely already provided the
model for this route, so `model` wouldn't be called in these cases.

あなたは、`afterModel`のロジックをなぜ`model`フックから帰ってくるPromiseのフルフィルハンドラーに入れることができないのか、と不思議に思うかもしれません。その理由は、上でも言及しましたが、`{{link-to}}`や`transitionTo`を通して開始されるページ遷移は、すでにこのRouteのためのModelを供給しているかもしれず、こうしたケースでは`model`フックは呼ばれないということにあります。

[See the API Docs for `afterModel`](/api/classes/Ember.Route.html#method_afterModel)

[`afterModel`のAPIドキュメントを参照してください。](http://emberjs.com/api/classes/Ember.Route.html#method_afterModel)


### More Resources
### さらなるリソース

- [Embercasts: Client-side Authentication Part 2](http://www.embercasts.com/episodes/client-side-authentication-part-2)
- [RC6 Blog Post describing these new features](/blog/2013/06/23/ember-1-0-rc6.html)

(The original document’s commit SHA1: 05df32d496dc0fe4925e60d730668a62d6f4b270)