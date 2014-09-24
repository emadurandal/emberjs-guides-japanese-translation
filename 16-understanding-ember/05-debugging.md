# DEBUGGING
# デバッグ

### Debugging Ember and Ember Data

Here are some tips you can use to help debug your Ember application.

あなたのEmberアプリケーションをデバッグするのを助けるために使えるいくつかのTipsがあります。

Also, check out the
[ember-extension](https://github.com/tildeio/ember-extension)
project, which adds an Ember tab to Chrome DevTools that allows you
to inspect Ember objects in your application.

また、[ember-extension](https://github.com/tildeio/ember-extension)プロジェクトもチェックしてください。このプロジェクトは、Chrome DevToolsに、あなたのアプリケーションの中のEmberオブジェクトを調査できるようにするEmberタブを追加するものです。

## Routing
## ルーティング

#### Log router transitions
#### ルーターの遷移をロギングする

```javascript
window.App = Ember.Application.create({
  // Basic logging, e.g. "Transitioned into 'post'"
  LOG_TRANSITIONS: true, 

  // Extremely detailed logging, highlighting every internal
  // step made while transitioning into a route, including
  // `beforeModel`, `model`, and `afterModel` hooks, and
  // information about redirects and aborted transitions
  LOG_TRANSITIONS_INTERNAL: true
});
```

#### View all registered routes
#### すべての登録されたルートを見る

```javascript
Ember.keys(App.Router.router.recognizer.names)
```

####  Get current route name / path
#### 現在のルートの名前／パスを取得する

Ember installs the current route name and path on your
app's `ApplicationController` as the properties
`currentRouteName` and `currentPath`. `currentRouteName`'s
value (e.g. `"comments.edit"`) can be used as the destination parameter of 
`transitionTo` and the `{{linkTo}}` Handlebars helper, while 
`currentPath` serves as a full descriptor of each
parent route that has been entered (e.g.
`"admin.posts.show.comments.edit"`).

Emberは現在のルートの名前とパスを、あなたのアプリケーションの`ApplicationController`に、`currentRouteName`と`currentPath`というプロパティとして、セットします。`currentRouteName`の値（例えば`"comments.edit"`）は`transitionTo`や`{{linkTo}}` Handlebarsヘルパーの目的地パラメータとして使用できます。一方、`currentPath`は今まで進入してきた各親のルートのフルの記述子を提供します（例えば、`"admin.posts.show.comments.edit"`）。

```javascript
// From within a Route
this.controllerFor("application").get("currentRouteName");
this.controllerFor("application").get("currentPath");

// From within a controller, after specifying `needs: ['application']`
this.get('controllers.application.currentRouteName');
this.get('controllers.application.currentPath');

// From the console:
App.__container__.lookup("controller:application").get("currentRouteName")
App.__container__.lookup("controller:application").get("currentPath")
```

## Views / Templates

#### Log view lookups
#### ビュールックアップのロギング

```javascript
window.App = Ember.Application.create({
  LOG_VIEW_LOOKUPS: true
});
```

#### Get the View object from its DOM Element's ID
#### DOM要素のIDから、ビューオブジェクトを取得する

```javascript
Ember.View.views['ember605']
```

#### View all registered templates
#### すべての登録されたテンプレートを見る

```javascript
Ember.keys(Ember.TEMPLATES)
```

#### Handlebars Debugging Helpers
#### Handlebarsのデバッグ用ヘルパー

```handlebars
{{debugger}}
{{log record}}
```

## Controllers

#### Log generated controller 
#### 生成されたコントローラーをロギングする

```javascript
window.App = Ember.Application.create({
  LOG_ACTIVE_GENERATION: true
});
```

## Ember Data

#### View ember-data's type maps
#### Ember Dataのタイプマップを表示する

```javascript
// all type maps in memory
App.__container__.lookup('store:main').typeMaps 

// specific type map in memory
App.__container__.lookup('store:main').typeMapFor(App.Color)

// map of id to record for all cached records for a type
App.__container__.lookup('store:main').typeMapFor(App.Color).idToRecord

// array of all cached records for a type
App.__container__.lookup('store:main').typeMapFor(App.Color).records

// grab a property off record id "33"
App.__container__.lookup('store:main').typeMapFor(App.Color).idToRecord["33"].get('color')
```

## Observers / Binding

#### See all observers for a object, key
#### オブジェクト、キーに対するすべてのオブザーバーを見る

```javascript
Ember.observersFor(comments, keyName);
```

#### Log object bindings
#### オブジェクトのバインディングをロギングする

```javascript
Ember.LOG_BINDINGS = true
```

## Miscellaneous
## その他、雑多なもの

#### Turn on resolver resolution logging
#### resolver resolution ロギングを有効にする

This option logs all the lookups that are done to the console. Custom objects
you've created yourself have a tick, and Ember generated ones don't.

このオプションは、全ての実行済みのルックアップをコンソールにロギングします。あなたが自分で作成したカスタムオブジェクトはチェックが付きますが、Emberが生成したオブジェクトはそうなりません。

It's useful for understanding which objects Ember is finding when it does a lookup
and which it is generating automatically for you.

これは、Emberがルックアップで見つけようとしているオブジェクトがどれであるか、また、Emberがあなたのために自動的に生成しているオブジェクトがどれであるかを理解するのに役立ちます。

```javascript
App = Ember.Application.create({
  LOG_RESOLVER: true
});
```

#### View an instance of something from the container
#### コンテナーから何かのインスタンスを見る

```javascript
App.__container__.lookup("controller:posts")
App.__container__.lookup("route:application")
```

#### Dealing with deprecations
#### 非推奨機能の扱い

```javascript
Ember.ENV.RAISE_ON_DEPRECATION = true
Ember.LOG_STACKTRACE_ON_DEPRECATION = true
```


#### Implement an Ember.onerror hook to log all errors in production
#### プロダクション環境において、すべてのエラーをロギングするために、Ember.onerrorフックを実装する

```javascript
Ember.onerror = function(error) {
    Em.$.ajax('/error-notification', {
      type: 'POST',
      data: {
        stack: error.stack,
        otherInformation: 'exception message'
      }
    });
}
```

#### Import the console
#### コンソールをインポートする

If you are using imports with Ember, be sure to import the console:

もしあなたがEmberでimpoetsを使っているなら、確実にコンソールをインポートしてください。

```javascript
Ember = {
  imports: {
    Handlebars: Handlebars,
    jQuery: $,
    console: window.console
  }
};
```

#### Errors within an `RSVP.Promise`
#### `RSVP.Promise`の内部のエラー

There are times when dealing with promises that it seems like any errors
are being 'swallowed', and not properly raised. This makes it extremely
difficult to track down where a given issue is coming from. Thankfully,
`RSVP` has a solution for this problem built in.

あらゆるエラーが「飲み込まれ」、正常に（エラーが）投げられないようなPromiseを扱う時があります。このようなケースでは、与えられた問題がどこからきているのか見つけ出すことが非常に困難です。ありがたいことに、`RSVP`はこの問題に対する解決策を内蔵しています。

You can provide an `onerror` function that will be called with the error
details if any errors occur within your promise. This function can be anything
but a common practice is to call `console.assert` to dump the error to the
console.

あなたは、何らかのエラーがPromiseの内部で起きた時に、そのエラーの詳細を引数にして呼び出される`onerror`関数を供給することができます。この関数では何をしてもよいですが、一般的な慣習では、コンソールにエラーをダンプする`console.assert`を呼びます。

```javascript
Ember.RSVP.on('error', function(error) {
  Ember.Logger.assert(false, error);
});
```

#### Errors within `Ember.run.later` ([Backburner.js](https://github.com/ebryn/backburner.js))
#### `Ember.run.later`の内部のエラー([Backburner.js](https://github.com/ebryn/backburner.js))

Backburner has support for stitching the stacktraces together so that you can
track down where an erroring `Ember.run.later` is being initiated from. Unfortunately,
this is quite slow and is not appropriate for production or even normal development.

Backburnerは複数のスタックトレースを一緒に繋げることをサポートしています。これによりあなたは、エラーを起こしている`Ember.run.later`がどこで初期化されているのか見つけ出すことができます。残念ながら、これは非常に低速で、実運用だけでなく、通常の開発にも向いてはいません。

To enable this mode you can set:
このモードを有効にするには、以下のように設定をします。

```javascript
Ember.run.backburner.DEBUG = true;
```

(The original document’s commit SHA1: 9ef38742bbd6d6693b97bbbf88f6e3776f5d2444)