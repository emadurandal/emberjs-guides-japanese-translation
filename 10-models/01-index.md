## Models

In Ember, every route has an associated model. This model is set by
implementing a route's `model` hook, by passing the model as an argument
to `{{link-to}}`, or by calling a route's `transitionTo()` method.

Emberでは、Routeごとに対応するModelがあります。このModelは、Routeの`model`フックを実装する、`{{link-to}}`の引数としてModelを渡す、または、Routeの`transitionTo()`メソッドを呼ぶ、ことでセットされます。

See [Specifying a Route's
Model](http://emberjs.com/guides/routing/specifying-a-routes-model) for more information
on setting a route's model.

RouteのModelをセットすることについて、より詳細を知るには[Specifying a Route's
Model](http://emberjs.com/guides/routing/specifying-a-routes-model)を参照してください。

For simple applications, you can get by using jQuery to load JSON data
from a server, then use those JSON objects as models.

シンプルなアプリケーションであれば、あなたはサーバーからJSONデータをロードするのにjQueryを使い、ModelとしてそれらのJSONオブジェクトを使うことができるでしょう。

However, using a model library that manages finding models, making
changes, and saving them back to the server can dramatically simplify
your code while improving the robustness and performance of your
application.

しかしながら、Modelを見つけ、変更を行い、サーバーに保存することを管理するModelライブラリを使うことで、アプリケーションの堅牢性とパフォーマンスを向上させながら、劇的にあなたのコードを簡素化することができます。

Many Ember apps use [Ember Data][emberdata] to handle this.
Ember Data is a library that integrates tightly with Ember.js to make it
easy to retrieve records from a server, cache them for performance,
save updates back to the server, and create new records on the client.

多くのEmberアプリケーションではModelを扱うために[Ember Data][emberdata]を使います。
Ember DataはEmber.jsと緊密に統合されたライブラリであり、サーバーからレコードを取得したり、パフォーマンスのためにそれらをキャッシュしたり、サーバーに対して更新を保存したり、クライアントにおいて新しいレコードを作ったりすることを容易にします。

Without any configuration, Ember Data can load and save records and
their relationships served via a RESTful JSON API, provided it follows
certain conventions.

何の設定もなしに、Ember DataはRESTful JSON APIを通してレコードとそれらのリレーションをロード・セーブすることができ、そしてそれらに次のようないくつかの規約を提供します。

If you need to integrate your Ember.js app with existing JSON APIs that
do not follow strong conventions, Ember Data is designed to be easily
configurable to work with whatever data your server returns.

もしあなたがEmber.jsアプリケーションと、強い規約に従っていない既存のJSON APIを統合する必要がある場合、Ember Dataはサーバーが返すデータがどんなものであっても動作するよう、簡単に設定できるように設計されています。

Ember Data is also designed to work with streaming APIs like
socket.io, Firebase, or WebSockets. You can open a socket to your server
and push changes to records into the store whenever they occur.

Ember Dataはsocket.io、Firebase、WebSocketsといったストリーミングAPIとも連携して動作できるよう設計されています。あなたはサーバーへのソケットを開き、レコードへの変更がいつ起きても、その変更をStoreにプッシュできます。

Currently, Ember Data ships as a separate library from Ember.js.  Until
Ember Data is included as part of the standard distribution, you can get
a copy of the latest passing build from
[emberjs.com/builds][builds]:

現在のところ、Ember DataはEmber.jsとは別のライブラリとしてリリースされています。Ember Dataが標準の配布の一部に含まれるようになるまで、あなたはビルドの通った最新のコピーを[emberjs.com/builds][builds]から入手することができます。

* [Development][development-build]
* [Minified][minified-build]

[emberdata]: https://github.com/emberjs/data
[builds]: http://emberjs.com/builds
[development-build]: http://builds.emberjs.com/canary/ember-data.js
[minified-build]: http://builds.emberjs.com/canary/ember-data.min.js

### Core Concepts
### コア・コンセプト

Learning to use Ember Data is easiest once you understand some of the
concepts that underpin its design.

Ember Dataの使い方を修得するのは、その設計に裏打ちされたいくつかのコンセプトを一度理解すれば簡単です。

#### Store

The **store** is the central repository of records in your application.
You can think of the store as a cache of all of the records available in
your app. Both your application's controllers and routes have access to this
shared store; when they need to display or modify a record, they will
first ask the store for it.

**Store**はアプリケーションのレコードの中央リポジトリです。Storeはあなたのアプリケーションで利用可能なすべてのレコードのキャッシュと考えることができます。あなたのアプリケーションのControllerとRouteのどちらも、この共有Storeへのアクセスができます。ControllerやRouteがレコードを表示または修正する必要が生じたとき、まず最初にStoreに問い合わせを行います。

This instance of `DS.Store` is created for you automatically and is shared
among all of the objects in your application.

この`DS.Store`のインスタンスはあなたのために自動的に生成され、アプリケーションのすべてのオブジェクトの間で共有されています。

You will use the store to retrieve records, as well to create new ones.
For example, we might want to find an `App.Person` model with the ID of
`1` from our route's `model` hook:

あなたはこのStoreをレコードの取得や作成に使うことになります。例えば、あなたがRouteの`model`フックから、`1`というIDで`App.Person`Modelを見つけたいとします。

```js
App.IndexRoute = Ember.Route.extend({
  model: function() {
    return this.store.find('person', 1);
  }
});
```

#### Models

A **model** is a class that defines the properties and behavior of the
data that you present to the user. Anything that the user expects to see
if they leave your app and come back later (or if they refresh the page)
should be represented by a model.

**Model**は、あなたがユーザーに見せようとしているデータのプロパティや動作を定義するクラスです。もしユーザーがあなたのアプリケーションを離れて、後に戻ってくる（あるいは彼らがページをリロードする）場合に、彼らが期待している動作はなんでも、Modelによってもたらされるべきです。

For example, if you were writing a web application for placing orders at
a restaurant, you might have models like `Order`, `LineItem`, and
`MenuItem`.

例えば、もしあなたがレストランに注文を行うWebアプリケーションを書いていた場合、あなたは`Order`、`LineItem`そして`MenuItem`のようなModelを持つかもしれません。

Fetching orders becomes very easy:

Orderを取得するのはとても簡単です。

```js
this.store.find('order');
```

Models define the type of data that will be provided by your server. For
example, a `Person` model might have a `firstName` attribute that is a
string, and a `birthday` attribute that is a date:

Modelはサーバーから提供されるデータの型を定義します。例えば、`Person`ModelはString型の`firstName`アトリビュートとDate型の`birthday`アトリビュートを持っているとします。

```js
App.Person = DS.Model.extend({
  firstName: DS.attr('string'),
  birthday:  DS.attr('date')
});
```

A model also describes its relationships with other objects. For
example, an `Order` may have many `LineItems`, and a `LineItem` may
belong to a particular `Order`.

Modelは他のオブジェクトとのリレーションも表現します。例えば、`Order`は複数の`LineItems`を持ち、そして`LineItem`は特定の`Order`に属します。
　
```js
App.Order = DS.Model.extend({
  lineItems: DS.hasMany('lineItem')
});

App.LineItem = DS.Model.extend({
  order: DS.belongsTo('order')
});
```

Models don't have any data themselves; they just define the properties and
behavior of specific instances, which are called _records_.

Modelはそれ自体では何のデータも持ちません。Modelはただ、そのプロパティと_Recored_と呼ばれる特定のインスタンスの動作を定義するだけです。

#### Records

A **record** is an instance of a model that contains data loaded from a
server. Your application can also create new records and save them back
to the server.

**Record**はサーバーから読み込まれたデータを含んだ、Modelのインスタンスです。あなたのアプリケーションは新しいレコードを作成し、それらをサーバーに保存することができます。

A record is uniquely identified by its model type and id.

レコードは、自信のModelの型とIDによってユニークに特定することができます。

For example, if you were writing a contact management app, you might
have a model called `Person`. An individual record in your app might
have a type of `Person` and an ID of `1` or `steve-buscemi`.

例えば、あなたが連絡先管理アプリケーションを書いていたとすると、`Person`というModelを持っているでしょう。あなたのアプリケーションの個別のレコードは`Person`の型と`1`または`steve-buscemi`というIDを持っています。

```js
this.store.find('person', 1); // => { id: 1, name: 'steve-buscemi' }
```

IDs are usually assigned by the server when you save them for the first
time, but you can also generate IDs client-side.

IDは通常、あなたがレコードを最初に保存する際に、サーバーから割り当てられます。しかし、あなたはIDをクライアントサイドで生成することもできます。

#### Adapter

An **adapter** is an object that knows about your particular server
backend and is responsible for translating requests for and changes to
records into the appropriate calls to your server.

**Adapter**は特定のサーバーバックエンドについて知っているオブジェクトで、リクエストやレコードへの変更を、そのサーバーに対する適切な呼び出しに変換する役割を担います。

For example, if your application asks for a `person` record with an ID
of `1`, how should Ember Data load it? Is it over HTTP or a WebSocket?
If it's HTTP, is the URL `/person/1` or `/resources/people/1`?

例えば、あなたのアプリケーションが`person`レコードを`1`のIDで検索したとしましょう。Ember Dataはどのようにしてそれをロードすべきでしょうか？　HTTPを通して？　またはWebSocketを通して？ もしそれがHTTPだったとして、URLは`/person/1` でしょうか、それとも `/resources/people/1`でしょうか？

The adapter is responsible for answering all of these questions.
Whenever your app asks the store for a record that it doesn't have
cached, it will ask the adapter for it. If you change a record and save
it, the store will hand the record to the adapter to send the
appropriate data to your server and confirm that the save was
successful.

Adapterはこれらの問いのすべてに答える責任を持ちます。あなたのアプリケーションが、キャッシュされていないレコードをStoreに問い合わせるときはいつでも、StoreはAdapterにそのことを問い合わせます。もしあなたがレコードを変更して保存しようとすれば、StoreはそのレコードをAdapterに持って行き、Adapterに適切なデータをサーバーに送信させ、そして保存が成功したことを確認します。

#### Serializer

A **serializer** is responsible for turning a raw JSON payload returned
from your server into a record object.

**Serializer**は、サーバーから返ってきた生のJSONデータをレコードオブジェクトに変換する役割を担います。

JSON APIs may represent attributes and relationships in many different
ways. For example, some attribute names may be `camelCased` and others
may be `under_scored`. Representing relationships is even more diverse:
they may be encoded as an array of IDs, an array of embedded objects, or
as foreign keys.

数あるJSON APIは、多くの異なる方法でアトリビュートとリレーションを表現します。例えば、いくつかのアトリビュートの名前が`camelCased`（キャメルケース）で、ほかは`under_scored`（アンダースコア区切り）だとしましょう。リレーションの表現はもっと多様です。それらはIDの配列としてエンコードされているかもしれないし、埋め込まれたオブジェクトの配列かもしれないし、外部キーとして実現されているかもしれません。

When the adapter gets a payload back for a particular record, it will
give that payload to the serializer to normalize into the form that
Ember Data is expecting.

Adapterがペイロード（データ）を特定のレコードに変換するとき、AdapterはEmber Dataが期待している形式に正規化するために、Serializerにそのペイロードを渡します。

While most people will use a serializer for normalizing JSON, because
Ember Data treats these payloads as opaque objects, there's no reason
they couldn't be binary data stored in a `Blob` or
[ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays/ArrayBuffer).

ほとんどの人がSerializerをJSONの正規化のために使いますが、Ember Dataはこれらのペイロードを不透明なオブジェクトとして扱うので、それらは`Blob`に保存されたバイナリデータや[ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays/ArrayBuffer)である可能性もあります。

#### Automatic Caching

The store will automatically cache records for you. If a record had already
been loaded, asking for it a second time will always return the same
object instance. This minimizes the number of round-trips to the
server, and allows your application to render its UI to the user as fast as
possible.

Storeはあなたのためにレコードを自動的にキャッシュします。もしレコードがすでにロードされていた場合、それへの二度目の問い合わせは、常に同じオブジェクトのインスタンスを返すでしょう。このことは、サーバーとのラウンドトリップの回数を最小化します。そして、あなたのアプリケーションができるだけ早く、ユーザーにそのUIをレンダリングできるようにします。

For example, the first time your application asks the store for a
`person` record with an ID of `1`, it will fetch that information from
your server.

例えば、あなたのアプリケーションが最初に`1`というIDで`person`レコードをStoreに問い合わせたとしましょう。

However, the next time your app asks for a `person` with ID `1`, the
store will notice that it had already retrieved and cached that
information from the server. Instead of sending another request for the
same information, it will give your application the same record it had
provided it the first time.  This feature—always returning the same
record object, no matter how many times you look it up—is sometimes
called an _identity map_.

しかしながら、次回にあなたのアプリケーションが`1`のIDで`person`を問い合わせると、Storeは、すでにそれが取得され、サーバーからの情報がキャッシュされていることに気づきます。同じ情報への他のリクエストを送信する代わりに、Storeは最初に提供したのと同じレコードをアプリケーションに与えます。この機能は、あなたが何度も何度も同じ問い合わせをしたとしても、常に、同じレコードオブジェクトを返します。この機能はときどき_identity map_と呼ばれます。

Using an identity map is important because it ensures that changes you
make in one part of your UI are propagated to other parts of the UI. It
also means that you don't have to manually keep records in sync—you can
ask for a record by ID and not have to worry about whether other parts
of your application have already asked for and loaded it.

identity mapを使うことは重要です。これは、UIの一部であなたが行った変更がUIの他の部分に伝播することを保証するからです。このことは、あなたが手動でレコード間を同期させる必要がないということも意味します。あなたはIDでレコードを問い合わせることができ、そしてアプリケーションの他の部分がすでにそれを問い合わせてロードしているか、心配する必要がないのです。

### Architecture Overview

The first time your application asks the store for a record, the store
sees that it doesn't have a local copy and requests it from your
adapter. Your adapter will go and retrieve the record from your
persistence layer; typically, this will be a JSON representation of the
record served from an HTTP server.

最初に、あなたのアプリケーションがStoreにレコードを問い合わせると、Storeはローカルコピーを持っていないことを確認し、Adapterからそれをリクエストします。Adapterは永続化レイヤーからレコードを取得します。この永続化レイヤーは、一般的には、HTTPサーバーからもたらされるレコードのJSON表現です。

![Diagram showing process for finding an unloaded record](http://guides.emberjs.com/v1.11.0/images/guides/models/finding-unloaded-record-step1-diagram.png)

As illustrated in the diagram above, the adapter cannot always return the 
requested record immediately. In this case, the adapter must make an 
_asynchronous_ request to the server, and only when that request finishes 
loading can the record be created with its backing data.

上の図で示しているように、Adapterは常に要求されたレコードを即座に返すとは限りません。このケースでは、Adapterは非同期リクエストをサーバーに対して行う必要があります。そして、そのリクエストがロードを終えた場合にのみ、その返ってきたデータを元にレコードを作成することができます。

Because of this asynchronicity, the store immediately returns a
_promise_ from the `find()` method. Similarly, any requests that the
store makes to the adapter also return promises.

この非同期性という理由から、Storeは`find()`メソッドからすぐに_promise_を返します。同様に、StoreがAdapterに対して作成したどのようなリクエストも、Promiseを返します。

Once the request to the server returns with a JSON payload for the
requested record, the adapter resolves the promise it returned to the
store with the JSON.

一度、サーバーへのリクエストが要求されたレコードのためのJSONペイロードを返したら、AdapterはPromiseを解決し、JSONをStoreに返します。

The store then takes that JSON, initializes the record with the
JSON data, and resolves the promise returned to your application
with the newly-loaded record.

StoreがJSONを取得したら、そのJSONデータを元にレコードを初期化します。そして新しくロードされたレコードを使って、あなたのアプリケーションに返ってきたPromiseを解決します。

![Diagram showing process for finding an unloaded record after the payload has returned from the server](http://guides.emberjs.com/v1.11.0/images/guides/models/finding-unloaded-record-step2-diagram.png)

Let's look at what happens if you request a record that the store
already has in its cache. 

もしあなたがすでにStoreがキャッシュしているレコードをリクエストしたときに、何が起きるか見てみましょう。

![Diagram showing process for finding an unloaded record after the payload has returned from the server](http://guides.emberjs.com/v1.11.0/images/guides/models/finding-loaded-record-diagram.png)

In this case, because the store already knew about the record, it
returns a promise that it resolves with the record immediately. It does
not need to ask the adapter (and, therefore, the server) for a copy
since it already has it saved locally.

このケースでは、Storeはすでにそのレコードについて知っているために、Storeは、そのレコードで即座にかいけつされるPromiseを返します。それはローカルにすでに保存されているのだから、そのコピーのためにAdapterに問い合わせる必要はありません（そして、それゆえ、サーバーに問い合わせる必要もありません）。

---

These are the core concepts you should understand to get the most out of
Ember Data. The following sections go into more depth about each of
these concepts, and how to use them together.

これらが、あなたがEmber Dataを最大限に活用するために理解すべきコア・コンセプトです。次からのセクションでは、これらのコンセプトそれぞれについてより詳細に立ち入り、そしてそれらを一緒にどのように使っていくかを見ていきます。

(The original document’s commit SHA1: 5f00e55cd9d20b061e90dfc91a712651e5f47da6)