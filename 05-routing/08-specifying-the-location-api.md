# SPECIFYING THE URL TYPE
# URLのタイプを指定する

By default the Router uses the browser's hash to load the starting state of your
application and will keep it in sync as you move around. At present, this relies
on a [hashchange](http://caniuse.com/hashchange) event existing in the browser.

標準では、Routerはあなたのアプリケーションの始動状態をロードするためにブラウザのハッシュを使い、あなたが移動に応じて状態を同期し続けます。今のところ、この振る舞いは、ブラウザに存在する[hashchange](http://caniuse.com/hashchange)イベントで実現しています。

Given the following router, entering `/#/posts/new` will take you to the `posts.new`
route.

次のようなルーターがあったとして、`/#/posts/new`に入ると`posts.new`ルートに移動することになります。

```javascript
App.Router.map(function() {
  this.resource('posts', function() {
    this.route('new');
  });
});
```

If you want `/posts/new` to work instead, you can tell the Router to use the browser's
[history](http://caniuse.com/history) API. 

もし代わりに`/posts/new`で動かしたいときは、Routerにブラウザーの[history](http://caniuse.com/history) APIを使うように命じることができます。

Keep in mind that your server must serve the Ember app at all the routes defined here.

あなたのサーバーは、ここで定義した全てのRouteでEmberアプリケーションを提供しなければならないことを心に留めておいてください。

```js
App.Router.reopen({
  location: 'history'
});
```

Finally, if you don't want the browser's URL to interact with your application
at all, you can disable the location API entirely. This is useful for
testing, or when you need to manage state with your Router, but temporarily
don't want it to muck with the URL (for example when you embed your
application in a larger page).

最後に、もしあなたがブラウザーのURLを、あなたのアプリケーションと全く相互作用させたくなかったら、location APIを完全に無効にすることができます。これはテストや、ルーターで状態を管理する必要があるものの、一時的にURLで状態をいじりたくない場合（例えば、より大きなページにあなたのアプリケーションを埋め込むときなど）に有用です。

```js
App.Router.reopen({
  location: 'none'
});
```

(The original document’s commit SHA1: a5f1cc4b8ab55f91d89fab4f661de38e2f87c058)