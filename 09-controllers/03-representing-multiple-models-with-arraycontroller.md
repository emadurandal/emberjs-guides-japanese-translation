# REPRESENTING MULTIPLE MODELS WITH ARRAYCONTROLLER
# ArrayControllerによる複数モデルの表現

You can use [Ember.ArrayController](http://emberjs.com/api/classes/Ember.ArrayController.html) to represent an array of models. To tell an
`ArrayController` which models to represent, set its `model` property
in your route's `setupController` method.

Modelの配列を表現するために、あなたは[Ember.ArrayController](http://emberjs.com/api/classes/Ember.ArrayController.html)を使うことができます。どのModelsを表現するか`ArrayController`に伝えるために、Routeの`setupController`メソッドの中で`ArrayController`の`model`プロパティをセットします。

You can treat an `ArrayController` just like its underlying array. For
example, imagine we want to display the current playlist. In our route,
we setup our `SongsController` to represent the songs in the playlist:

あなたは`ArrayController`を、その内在的な配列のような感じで扱うことができます。例えば、現在のプレイリストを表示したいと仮定しましょう。Routeでは、プレイリストの曲を表現するために`SongsController`をセットアップします。

```javascript
App.SongsRoute = Ember.Route.extend({
  setupController: function(controller, playlist) {
    controller.set('model', playlist.get('songs'));
  }
});
```

In the `songs` template, we can use the `{{#each}}` helper to display
each song:

`songs`テンプレートでは、各曲を表示するための`{{#each}}`ヘルパーを使うことができます。

```handlebars
<h1>Playlist</h1>

<ul>
  {{#each song in model}}
    <li>{{song.name}} by {{song.artist}}</li>
  {{/each}}
</ul>
```

You can use the `ArrayController` to collect aggregate information about
the models it represents. For example, imagine we want to display the
number of songs that are over 30 seconds long. We can add a new computed
property called `longSongCount` to the controller:

（コントローラーが表現した）Modelについての総計の情報を集めるために`ArrayController`を使うことができます。例えば、３０秒以上の長さの曲の数を表示したいと仮定しましょう。私たちは、Controllerに`longSongCount`という新しいComputed Propertyを加えることができます。

```javascript
App.SongsController = Ember.ArrayController.extend({
  longSongCount: function() {
    var longSongs = this.filter(function(song) {
      return song.get('duration') > 30;
    });
    return longSongs.get('length');
  }.property('@each.duration')
});
```

Now we can use this property in our template:

これで、私たちはこのプロパティをテンプレートで使うことができます。

```handlebars
<ul>
  {{#each song in model}}
    <li>{{song.name}} by {{song.artist}}</li>
  {{/each}}
</ul>

{{longSongCount}} songs over 30 seconds.
```

### Sorting
### ソート

The `Ember.ArrayController` uses the [Ember.SortableMixin](http://emberjs.com/api/classes/Ember.SortableMixin.html) to allow sorting
of content. There are two properties that can be set in order to set up sorting:

`Ember.ArrayController`はコンテンツをソートできるようにするために、[Ember.SortableMixin](http://emberjs.com/api/classes/Ember.SortableMixin.html)を使っています。ソーティングを設定するための２つのプロパティがあります。

```javascript
App.SongsController = Ember.ArrayController.extend({
  sortProperties: ['name', 'artist'],
  sortAscending: true // false for descending
});
```


### Item Controller

It is often useful to specify a controller to decorate individual items in
the `ArrayController` while iterating over them. This can be done by
creating an `ObjectController`:

`ArrayController`をイテレートしている間、それらの中の個別の項目を装飾するために、Controllerを指定することはしばしば有用です。これは `ObjectController`を作成することで行えます。

```javascript
App.SongController = Ember.ObjectController.extend({
  fullName: function() {
 
    return this.get('name') + ' by ' + this.get('artist');
 
  }.property('name', 'artist')
});
```
 
Then, the `ArrayController` `itemController` property must be set to
the decorating controller.

そして、`ArrayController` の `itemController`プロパティに、装飾する側のControllerをセットする必要があります。

```javascript
App.SongsController = Ember.ArrayController.extend({
  itemController: 'song'
});
```

```handlebars
{{#each item in controller}}
  <li>{{item.fullName}}</li>
{{/each}}
```

or you could setup the `itemController` directly in the template:

または、テンプレートの中で直接、`itemController`をセットアップすることができます。

```javascript
App.SongsController = Ember.ArrayController.extend({
});
```

```handlebars
{{#each item in controller itemController="song"}}
  <li>{{item.fullName}}</li>
{{/each}}
```

(The original document’s commit SHA1: ec0c79f78dad7b464efc5cb009ecf045a51f6a44)