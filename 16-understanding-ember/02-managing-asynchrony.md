# MANAGING ASYNCHRONY
# 非同期性の管理

Many Ember concepts, like bindings and computed properties, are designed
to help manage asynchronous behavior.

バイディングやComputed Propertiesのような、多くのEmberのコンセプトは、非同期の動作を管理するのを助けるように設計されています。

### Without Ember
### Emberを使わない場合

We'll start by taking a look at ways to manage asynchronous behavior
using jQuery or event-based MVC frameworks.

まず、jQueryやイベントベースのMVCフレームワークを使って、非同期の動作を管理する方法について見てみましょう。

Let's use the most common asynchronous behavior in a web application,
making an Ajax request, as an example. The browser APIs for making Ajax
requests provide an asynchronous API. jQuery's wrapper does as well:

Webアプリケーションにおいて最も一般的な非同期の動作を使ってみましょう。一例として、Ajaxリクエストを作ります。Ajaxリクエストを行うため、ブラウザのAPIは、非同期のAPIを提供します。jQueryのラッパーも同様のことを行います。

```javascript
jQuery.getJSON('/posts/1', function(post) {
  $("#post").html("<h1>" + post.title + "</h1>" +
    "<div>" + post.body + "</div>");
});
```

In a raw jQuery application, you would use this callback to make
whatever changes you needed to make to the DOM.

生のjQueryアプリケーションでは、DOMを作るために必要な変更をするためには、このコールバックを使うことになるでしょう。

When using an event-based MVC framework, you move the logic out of the
callback and into model and view objects. This improves things, but
doesn't get rid of the need to explicitly deal with asynchronous
callbacks:

イベントベースのMVCフレームワークを使っているときは、ロジックをコールバックから、ModelとViewオブジェクトに移します。これは事態を改善しますが、しかし非同期コールバックを明示的に扱う必要性を取り除けるわけではありません。

```javascript
Post = Model.extend({
  author: function() {
    return [this.salutation, this.name].join(' ')
  },

  toJSON: function() {
    var json = Model.prototype.toJSON.call(this);
    json.author = this.author();
    return json;
  }
});

PostView = View.extend({
  init: function(model) {
    model.bind('change', this.render, this);
  },

  template: _.template("<h1><%= title %></h1><h2><%= author %></h2><div><%= body %></div>"),

  render: function() {
    jQuery(this.element).html(this.template(this.model.toJSON());
    return this;
  }
});

var post = Post.create();
var postView = PostView.create({ model: post });
jQuery('#posts').append(postView.render().el);

jQuery.getJSON('/posts/1', function(json) {
  // set all of the JSON properties on the model
  post.set(json);
});
```

This example doesn't use any particular JavaScript library beyond
jQuery, but its approach is typical of event-driven MVC frameworks. It
helps organize the asynchronous events, but asynchronous behavior is
still the core programming model.

この例では、jQueryを超えるような特定のJavaScriptライブラリを使用することはありませんが、そのアプローチはイベント駆動MVCフレームワークの典型です。これは、非同期イベントを整理するのには役立ちますが、非同期動作が依然としてコアプログラミングモデルとなっています。

### Ember's Approach

In general, Ember's goal is to eliminate explicit forms of asynchronous
behavior. As we'll see later, this gives Ember the ability to coalesce
multiple events that have the same result.

一般的に、Emberの目標は、非同期動作の明示的な形を排除することです。後に見ていきますが、この目標はEmberに、同じ結果をもつ複数のイベントを合体させる能力を持たせます。

It also provides a higher level of abstraction, eliminating the need to
manually register and unregister event listeners to perform most common
tasks.

このことはまた、高い水準の抽象化も提供します。最も一般的なタスクを実行するために、手動でイベントリスナを登録したり解除したりする必要をなくすのです。

You would normally use ember-data for this example, but let's see how
you would model the above example using jQuery for Ajax in Ember.

こうした例では、通常はember-dataを使うのですが、まずは、Emberにおいて、AjaxのためにjQueryを使って、上記の例をいかにモデル化するのか見てみましょう。

```javascript
App.Post = Ember.Object.extend({
  
});

App.PostController = Ember.ObjectController.extend({
  author: function() {
    return [this.get('salutation'), this.get('name')].join(' ');
  }.property('salutation', 'name')
});

App.PostView = Ember.View.extend({
  // the controller is the initial context for the template
  controller: null,
  template: Ember.Handlebars.compile("<h1>{{title}}</h1><h2>{{author}}</h2><div>{{body}}</div>")
});

var post = App.Post.create();
var postController = App.PostController.create({ model: post });

App.PostView.create({ controller: postController }).appendTo('body');

jQuery.getJSON("/posts/1", function(json) {
  post.setProperties(json);
});
```

In contrast to the above examples, the Ember approach eliminates the
need to explicitly register an observer when the `post`'s properties
change.

先ほどの例とは対照的に、Emberのアプローチは`post`のプロパティが変更されたとき、Observerを明示的に登録する必要性をなくしています。

The `{{title}}`, `{{author}}` and `{{body}}` template elements are bound
to those properties on the `PostController`. When the `PostController`'s
model changes, it automatically propagates those changes to the DOM.

`{{title}}`、`{{author}}`、`{{body}}`テンプレート要素は`PostController`の対応するプロパティにバインドされています。`PostController`のModelが変更されたとき、`PostController`は自動的にそれらの変更をDOMに伝えます。

Using a computed property for `author` eliminated the need to explicitly
invoke the computation in a callback when the underlying property
changed.

`author`のためにComputed Propertyを使うことで、依存しているプロパティが変更されたときに、コールバックの中で計算を明示的に呼び出す必要性をなくしています。

Instead, Ember's binding system automatically follows the trail from the
`salutation` and `name` set in the `getJSON` callback to the computed
property in the `PostController` and all the way into the DOM.

代わりに、Emberのバインディングシステムは自動的に、`getJSON`コールバックの中の`salutation`と`name`のセットから、`PostController`のComputed Property、そしてDOMまで追跡を行います。

### Benefits
### 利点

Because Ember is usually responsible for propagating changes, it can
guarantee that a single change is only propagated one time in response
to each user event.

Emberは通常、変更を伝播する責任を負っているので、各ユーザーイベントに応えて、単一の変更が一回だけ伝播されることが保証されています。

Let's take another look at the `author` computed property.

`author` Computed Propertyをもう一度見てみましょう。

```javascript
App.PostController = Ember.ObjectController.extend({
  author: function() {
    return [this.get('salutation'), this.get('name')].join(' ');
  }.property('salutation', 'name')
});
```

Because we have specified that it depends on both `salutation` and
`name`, changes to either of those two dependencies will invalidate the
property, which will trigger an update to the `{{author}}` property in
the DOM.

私たちが、このComputed Propertyが`salutation`と`name`の両方に依存することを指定しているため、それら二つの依存先プロパティのどちらかに対する変更は、そのプロパティ（Computed Property）を取り消します。そしてそれはDOMの`{{author}}`プロパティの更新をトリガーします。

Imagine that in response to a user event, I do something like this:

ユーザーイベントへの反応として、このような何かを考えてみましょう。

```javascript
post.set('salutation', "Mrs.");
post.set('name', "Katz");
```

You might imagine that these changes will cause the computed property to
be invalidated twice, causing two updates to the DOM. And in fact, that
is exactly what would happen when using an event-driven framework.

あなたは、これらの変更はComputed Propertyを二回取り消し、DOMへの２回の更新を行うと予想するかもしれません。そして実際、イベント駆動フレームワークを使っているとき、それはまさしく起きることなのです。

In Ember, the computed property will only recompute once, and the DOM
will only update once.

Emberでは、Computed Propertyはただ一回だけ再計算し、DOMは一度だけ更新されます。

How?

どうやって？

When you make a change to a property in Ember, it does not immediately
propagate that change. Instead, it invalidates any dependent properties
immediately, but queues the actual change to happen later.

Emberでプロパティへの変更をあなたが行うとき、その変更はすぐには伝播しません。代わりに、依存しているComputed Propertyはすぐに取り消されますが、実際の変更は（あとで起こすために）キューに入れられます。

Changing both the `salutation` and `name` properties invalidates the
`author` property twice, but the queue is smart enough to coalesce those
changes.

`salutation`と`name`プロパティの両方の変更は、`author`プロパティを二度取り消しますが、キューは賢いので、これらの変更を合体させるのです。

Once all of the event handlers for the current user event have finished,
Ember flushes the queue, propagating the changes downward. In this case,
that means that the invalidated `author` property will invalidate the
`{{author}}` in the DOM, which will make a single request to recompute
the information and update itself once.

一度、現在のユーザーイベントのための全てのイベントハンドラーの実行が完了すると、Emberはキューをフラッシュし、変更を下に伝播させます。この場合、それは、取り消された`author`プロパティがDOMの`{{author}}`を取り消すことを意味します。そして、情報を再計算し、自身を一度更新するための単一のリクエストを作成します。

**This mechanism is fundamental to Ember.** In Ember, you should always
assume that the side-effects of a change you make will happen later. By
making that assumption, you allow Ember to coalesce repetitions of the
same side-effect into a single call.

**このメカニズムはEmberの基本です。** Emberでは、あなたはあなたが行った変更の副作用が後に起こることを常に想定すべきです。その仮定をした上で、あなたはEmberに、同じ副作用の繰り返しを単一の呼び出しに合体させるようにするのです。

In general, the goal of evented systems is to decouple the data
manipulation from the side effects produced by listeners, so you
shouldn't assume synchronous side effects even in a more event-focused
system. The fact that side effects don't propagate immediately in Ember
eliminates the temptation to cheat and accidentally couple code together
that should be separate.

一般には、イベント駆動システムの目標は、リスナーによって生み出される副作用からデータ操作を切り離すことです。なので、あなたはよりイベントにフォーカスされたシステムにおいてさえ、同期的な副作用を想定すべきではありません。Emberでは副作用はすぐには伝播されないという事実により、ごまかしをしようとしたり、分離すべきコードをうっかりくっつけたりする誘惑がなくなります。

### Side-Effect Callbacks
### 副作用コールバック

Since you can't rely on synchronous side-effects, you may be wondering
how to make sure that certain actions happen at the right time.

同期的副作用に頼ることができないので、あなたはあるアクションを正しい時間に、どう確実に実行させられるのか不思議に思うかも知れません。

For example, imagine that you have a view that contains a button, and
you want to use jQuery UI to style the button. Since a view's `append`
method, like everything else in Ember, defers its side-effects, how can
you execute the jQuery UI code at the right time?

たとえば、あなたはボタンを含んでいるViewを持っており、そのボタンをスタイル付けするためにjQuery UIを使いたいとします。Emberの他のすべてのように、ビューの`append`メソッドはその副作用を引き延ばそうとします。あなたはどうやってjQuery UIのコードを正しい時間に実行させることができるでしょう？

The answer is lifecycle callbacks.

その答えは、ライフサイクルコールバックです。

```javascript
App.Button = Ember.View.extend({
  tagName: 'button',
  template: Ember.Handlebars.compile("{{view.title}}"),

  didInsertElement: function() {
    this.$().button();
  }
});

var button = App.Button.create({
  title: "Hi jQuery UI!"
}).appendTo('#something');
```

In this case, as soon as the button actually appears in the DOM, Ember
will trigger the `didInsertElement` callback, and you can do whatever
work you want.

この場合では、ボタンが実際にDOMに現れたらすぐに、Emberは`didInsertElement`コールバックをトリガします。そしてあなたは、望むあらゆる仕事を行うことができます。

The lifecycle callbacks approach has several benefits, even if we didn't
have to worry about deferred insertion.

たとえ、私たちが遅延された挿入について心配する必要がなかったとしても、このライフサイクルコールバックのアプローチはいくつかの利点があります。

*First*, relying on synchronous insertion means leaving it up to the
caller of `appendTo` to trigger any behavior that needs to run
immediately after appending. As your application grows, you may find
that you create the same view in many places, and now need to worry
about that concern everywhere.

*まず最初に*、同期的挿入に頼るということは、`appendTo`を呼び出した者に、追加した後すぐに実行する必要のあるあらゆる動作をトリガすることを任せたことを意味します。あなたのアプリケーションが大きくなるにつれ、あなたは、多くの場所で同じビューを作成し、あらゆる場所でその懸念を心配する必要が出てくることに気付くかもしれません。

The lifecycle callback eliminates the coupling between the code that
instantiates the view and its post-append behavior. In general, we find
that making it impossible to rely on synchronous side-effects leads to
better design in general.

ライフサイクルコールバックは、ビューをインスタンス化するコードと、その追加した後の動作の結合をなくします。一般的には、同期的副作用に頼ることを不可能にすることは、よりよい設計を導くということに、私たちは気付きました。

*Second*, because everything about the lifecycle of a view is inside the
view itself, it is very easy for Ember to re-render parts of the DOM
on-demand.

*第二に*、ビューのライフサイクルについてのすべてがView自身の内部にあるので、Emberが必要に応じてDOMの部分を再レンダリングすることは非常に簡単なのです。

For example, if this button was inside of an `{{#if}}` block, and Ember
needed to switch from the main branch to the `else` section, Ember can
easily instantiate the view and call the lifecycle callbacks.

たとえば、このボタンが`{{#if}}`ブロックの内部にあり、そしてEmberがメインの分岐から`else`セクションに切り替える必要があったなら、Emberは簡単にビューをインスタンス化し、ライフサイクルコールバックを呼ぶことができます。

Because Ember forces you to define a fully-defined view, it can take
control of creating and inserting views in appropriate situations.

Emberはあなたに十分に明確化されたビューを定義することを強いるので、Emberは最適な状況で、ビューの生成と挿入の制御を行うことができます。

This also means that all of the code for working with the DOM is in a
few sanctioned parts of your application, so Ember has more freedom in
the parts of the render process outside of these callbacks.

このことはまた、DOMを扱う全てのコードは、あなたのアプリケーションのいくつかの認可された部分であることも意味します。なので、Emberは、これらのコールバックの外側のレンダリングプロセスの一部では、より多くの自由を持っているのです。

### Observers

In some rare cases, you will want to perform certain behavior after a
property's changes have propagated. As in the previous section, Ember
provides a mechanism to hook into the property change notifications.

いくつかのまれなケースでは、あなたはプロパティの変化が伝播した後に、ある動作を実行したいと思うかもしれません。前節と同様に、Emberはプロパティの変更通知をフックするためのメカニズムを提供します。

Let's go back to our salutation example.
salutationの例に戻りましょう。

```javascript
App.PostController = Ember.ObjectController.extend({
  author: function() {
    return [this.get('salutation'), this.get('name')].join(' ');
  }.property('salutation', 'name')
});
```

If we want to be notified when the author changes, we can register an
observer. Let's say that the view object wants to be notified:

authorが変更された時に通知を受けたい場合は、私たちはObserverを登録することができます。Viewオブジェクトが通知されることを望んでいるとしましょう。

```javascript
App.PostView = Ember.View.extend({
  controller: null,
  template: Ember.Handlebars.compile("<h1>{{title}}</h1><h2>{{author}}</h2><div>{{body}}</div>"),

  authorDidChange: function() {
    alert("New author name: " + this.get('controller.author'));
  }.observes('controller.author')
});
```

Ember triggers observers after it successfully propagates the change. In
this case, that means that Ember will only call the `authorDidChange`
callback once in response to each user event, even if both of `salutation`
and `name` changed.

Emberは、（`author`が）正常に変更を伝播した後、Observerをトリガします。この場合、このことは、Emberは各ユーザーイベントに応じて、仮に`salutation`と`name`の両方が変更されたとしても、`authorDidChange`コールバックを一回だけ呼び出すことを意味します。

This gives you the benefits of executing code after the property has
changed, without forcing all property changes to be synchronous. This
basically means that if you need to do some manual work in response to a
change in a computed property, you get the same coalescing benefits as
Ember's binding system.

このことはあなたに、すべてのプロパティの変化を同期させることを強いることなく、プロパティが変化した後にコードを実行する利点を与えてくれます。このことは基本的に、もしあなたがComputed Propertyにおける変更に応じてなんらかの手動の作業を行う必要があるなら、Emberのバインディングシステムと同じ合体のメリットを得られることを意味します。

Finally, you can also register observers manually, outside of an object
definition:

最後に、あなたはオブジェクトの定義の外で、Observerを手動で登録することもできます。

```javascript
App.PostView = Ember.View.extend({
  controller: null,
  template: Ember.Handlebars.compile("<h1>{{title}}</h1><h2>{{author}}</h2><div>{{body}}</div>"),

  didInsertElement: function() {
    this.addObserver('controller.author', function() {
      alert("New author name: " + this.get('controller.author'));
    });
  }
});
```

However, when you use the object definition syntax, Ember will
automatically tear down the observers when the object is destroyed. For
example, if an `{{#if}}` statement changes from truthy to falsy, Ember
destroys all of the views defined inside the block. As part of that
process, Ember also disconnects all bindings and inline observers.

しかしながら、あなたがオブジェクト定義の文法を使っているとき、Emberはオブジェクトが破棄されたときに、Observerを自動的に解体します。たとえば、もし`{{#if}}`文が真から偽に変わったら、Emberはブロックの内部で定義された全てのビューを削除します。その過程の一部として、EmberはすべてのバインディングとインラインのObserverの接続の解除も行います。

If you define an observer manually, you need to make sure you remove it.
In general, you will want to remove observers in the opposite callback
to when you created it. In this case, you will want to remove the
callback in `willDestroyElement`.

もしあなたがObserverを手動で定義したなら、自分でそれを確実に削除する必要があります。一般的に、あなたはそれを作成したときと反対のコールバックでObserverを削除したいと思うでしょう。この場合、あなたは`willDestroyElement`の中でコールバックを削除したいと思うでしょう。

```javascript
App.PostView = Ember.View.extend({
  controller: null,
  template: Ember.Handlebars.compile("<h1>{{title}}</h1><h2>{{author}}</h2><div>{{body}}</div>"),

  didInsertElement: function() {
    this.addObserver('controller.author', function() {
      alert("New author name: " + this.get('controller.author'));
    });
  },

  willDestroyElement: function() {
    this.removeObserver('controller.author');
  }
});
```

If you added the observer in the `init` method, you would want to tear
it down in the `willDestroy` callback.

もしあなたが`init`メソッドの中にObserverを追加したなら、`willDestory`コールバックの中でObserverを削除したいと思うでしょう。

In general, you will very rarely want to register a manual observer in
this way. Because of the memory management guarantees, we strongly
recommend that you define your observers as part of the object
definition if possible.

一般的に、あなたがこの方法で手動のObserverを登録したいと思うことは非常にまれでしょう。メモリ管理が保証されているという理由から、私たちは、可能であるなら、オブジェクト定義の一部としてObserverを定義することを強く勧めます。

### Routing

There's an entire page dedicated to managing async within the Ember
Router: [Asynchronous Routing](http://emberjs.com/guides/routing/asynchronous-routing)

EmberのRouterの内部の非同期を管理することについて、非常に詳しく載っている完全なページがあります：[Asynchronous Routing](http://emberjs.com/guides/routing/asynchronous-routing)

(The original document’s commit SHA1: d92a30b3b8688310e8adfd60ea4a88557ba4a574)