# INTRODUCTION
# イントロダクション

## Routing
## ルーティング

As users interact with your application, it moves through many
different states. Ember.js gives you helpful tools for managing
that state in a way that scales with your application.

ユーザーがあなたのアプリケーションを利用すると、アプリケーションは多くの異なる状態の間を移動します。Ember.jsは、あなたのアプリケーションに対応するやり方で、状態を管理するのに役立つツールをあなたに提供します。

To understand why this is important, imagine we are writing a web app
for managing a blog. At any given time, we should be able to answer
questions like: _Is the user currently logged in? Are they an admin
user? What post are they looking at? Is the settings screen open?  Are
they editing the current post?_

このことがなぜ重要なのか理解するために、私たちがブログを管理するWebアプリケーションを書いていると想像しましょう。どんなときでも、私たちは次のような質問に答えられなければなりません。_ユーザーは今ログインしているか？　彼らは管理者ユーザーか？　彼らが見ているのはどの投稿？　設定画面は開いているか？　彼らは現在の投稿を編集中か？_

In Ember.js, each of the possible states in your application is
represented by a URL. Because all of the questions we asked above—
_Are we logged in?  What post are we looking at?_ —are encapsulated by
route handlers for the URLs, answering them is both simple and accurate.

Ember.jsでは、あなたのアプリケーションでとりうるそれぞれの状態を、URLで表現します。私たちが先ほど尋ねた全ての質問（_私たちはログインしているか？　私たちが見ているのはどの投稿か？_）は、URLのためのRouteハンドラーによってカプセル化されるため、それらへの回答はシンプルかつ正確になるのです。

At any given time, your application has one or more _active route
handlers_. The active handlers can change for one of two reasons:

どんなときでも、あなたのアプリケーションは一つまたはそれ以上のアクティブなRouteハンドラーを持ちます。アクティブなハンドラーは次の２つの理由のうちのいずれかにより、変化します。

1. The user interacted with a view, which generated an event that caused
   the URL to change.
   
   ユーザーがビューを操作し、ViewがURLを変更させるイベントを生成した。
   
2. The user changed the URL manually (e.g., via the back button), or the
   page was loaded for the first time.
   
   ユーザーがURLを手動で変更した（例えば、バックボタンを押すことなどにより）。またはページが初めて読み込まれた。

When the current URL changes, the newly active route handlers may do one
or more of the following:

現在のURLが変化するとき、新しいアクティブなRouteハンドラーは、次のうち１つまたは複数を実行するかもしれません。

1. Conditionally redirect to a new URL.

      条件付きで、新しいURLにリダイレクトする。

2. Update a controller so that it represents a particular model.
   
   Controllerを更新する。それにより、Controllerが特定のモデルを表示する。
   
3. Change the template on screen, or place a new template into an
   existing outlet.
   
   画面のテンプレートを変更する。あるいは、既存のOutletに新しいテンプレートを配置する。

### Logging Route Changes
### Routeの変更のロギング

As your application increases in complexity, it can be helpful to see exactly what is going on with the router. To have Ember write out transition events to the log, simply modify your `Ember.Application`:

あなたのアプリケーションが複雑さを増してきたら、Routerで何が起きているのか正確に知ることは有益です。Emberに遷移イベントをログに出力させるようにするには、単に`Ember.Application`を次のように変更します。

```javascript
App = Ember.Application.create({
  LOG_TRANSITIONS: true
});
```

### Specifying a Root URL
### ルートURLの指定

If your Ember application is one of multiple web applications served from the same domain, it may be necessary to indicate to the router what the root URL for your Ember application is. By default, Ember will assume it is served from the root of your domain.

もし、あなたのEmberアプリケーションが同じドメインから提供されている複数のWebアプリケーションのうちの一つだった場合、あなたのEmberアプリケーションのルートURLをRouterに示す必要があります。標準では、Emberはアプリケーションがあなたのドメインのルートから提供されていると仮定します。

If for example, you wanted to serve your blogging application from www.emberjs.com/blog/, it would be necessary to specify a root URL of `/blog/`.

例えば、もしあなたのブログアプリケーションを www.emberjs.com/blog/ から提供したい場合は、ルートURLを`/blog/`に指定する必要があります。

This can be achieved by setting the rootURL on the router:

これは、RouterにてrootURLをセットすることで可能です。

```js
App.Router.reopen({
  rootURL: '/blog/'
});
```

(The original document’s commit SHA1: ecdbf49baebc3e6a4ed81d8bc0e16e3285da4189)